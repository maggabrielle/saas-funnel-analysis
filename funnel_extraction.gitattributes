-- =============================================================================
-- SaaS Funnel Conversion Analysis — SQL Extraction Layer
-- Author: Gabrielle Magalhães Soares
-- Description: Extracts and transforms raw user event data into a structured
--              funnel model for conversion analysis by segment and channel.
-- Dialect: BigQuery-compatible (also runs in PostgreSQL with minor adjustments)
-- =============================================================================


-- -----------------------------------------------------------------------------
-- CTE 1: base_users
-- Raw source table with one row per user.
-- In production, this would pull from the events or users table in the warehouse.
-- -----------------------------------------------------------------------------
WITH base_users AS (
    SELECT
        user_id,
        segment,
        channel,
        plan,
        visited_landing_page,
        started_trial,
        activated,
        used_key_feature,
        converted_to_paid
    FROM `saas_analytics.funnel_data`
),


-- -----------------------------------------------------------------------------
-- CTE 2: funnel_stages
-- Maps binary flags to a named funnel stage for each user.
-- Uses CASE to assign the deepest stage reached, enabling drop-off calculation.
-- -----------------------------------------------------------------------------
funnel_stages AS (
    SELECT
        user_id,
        segment,
        channel,
        plan,

        -- Deepest stage reached (used for funnel volume per stage)
        CASE
            WHEN converted_to_paid   = 1 THEN 'converted_to_paid'
            WHEN used_key_feature    = 1 THEN 'used_key_feature'
            WHEN activated           = 1 THEN 'activated'
            WHEN started_trial       = 1 THEN 'started_trial'
            WHEN visited_landing_page = 1 THEN 'visited_landing_page'
            ELSE 'no_activity'
        END AS deepest_stage,

        -- Stage order for sorting
        CASE
            WHEN converted_to_paid   = 1 THEN 5
            WHEN used_key_feature    = 1 THEN 4
            WHEN activated           = 1 THEN 3
            WHEN started_trial       = 1 THEN 2
            WHEN visited_landing_page = 1 THEN 1
            ELSE 0
        END AS stage_order,

        -- Individual stage flags (kept for segmented analysis)
        visited_landing_page,
        started_trial,
        activated,
        used_key_feature,
        converted_to_paid

    FROM base_users
),


-- -----------------------------------------------------------------------------
-- CTE 3: funnel_volume
-- Aggregates total users at each stage of the funnel.
-- Each column counts users who reached that stage or beyond.
-- -----------------------------------------------------------------------------
funnel_volume AS (
    SELECT
        COUNT(*)                                        AS total_users,
        SUM(visited_landing_page)                       AS visited_lp,
        SUM(started_trial)                              AS started_trial,
        SUM(activated)                                  AS activated,
        SUM(used_key_feature)                           AS used_key_feature,
        SUM(converted_to_paid)                          AS converted_to_paid
    FROM funnel_stages
),


-- -----------------------------------------------------------------------------
-- CTE 4: conversion_rates
-- Calculates step-by-step conversion rates and cumulative conversion from top.
-- SAFE_DIVIDE avoids division by zero (BigQuery); use NULLIF in PostgreSQL.
-- -----------------------------------------------------------------------------
conversion_rates AS (
    SELECT
        visited_lp,
        started_trial,
        activated,
        used_key_feature,
        converted_to_paid,

        -- Step-to-step conversion rates
        ROUND(SAFE_DIVIDE(started_trial,    visited_lp)      * 100, 1) AS lp_to_trial_pct,
        ROUND(SAFE_DIVIDE(activated,        started_trial)   * 100, 1) AS trial_to_activation_pct,
        ROUND(SAFE_DIVIDE(used_key_feature, activated)       * 100, 1) AS activation_to_key_feature_pct,
        ROUND(SAFE_DIVIDE(converted_to_paid, used_key_feature) * 100, 1) AS key_feature_to_paid_pct,

        -- Overall conversion: LP visitors who became paying customers
        ROUND(SAFE_DIVIDE(converted_to_paid, visited_lp)    * 100, 1) AS overall_conversion_pct

    FROM funnel_volume
),


-- -----------------------------------------------------------------------------
-- CTE 5: conversion_by_segment
-- Breaks down funnel performance by company segment (SMB, Mid-Market, Enterprise).
-- Enables comparison of conversion rates across customer profiles.
-- -----------------------------------------------------------------------------
conversion_by_segment AS (
    SELECT
        segment,
        COUNT(*)                                                         AS total_users,
        SUM(visited_landing_page)                                        AS visited_lp,
        SUM(started_trial)                                               AS started_trial,
        SUM(converted_to_paid)                                           AS converted_to_paid,

        ROUND(SAFE_DIVIDE(SUM(started_trial), SUM(visited_landing_page)) * 100, 1) AS lp_to_trial_pct,
        ROUND(SAFE_DIVIDE(SUM(activated),     SUM(started_trial))        * 100, 1) AS trial_to_activation_pct,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), SUM(visited_landing_page)) * 100, 1) AS overall_conversion_pct

    FROM funnel_stages
    GROUP BY segment
    ORDER BY overall_conversion_pct DESC
),


-- -----------------------------------------------------------------------------
-- CTE 6: conversion_by_channel
-- Breaks down funnel performance by acquisition channel.
-- Identifies which channels drive users with the highest conversion potential.
-- -----------------------------------------------------------------------------
conversion_by_channel AS (
    SELECT
        channel,
        COUNT(*)                                                           AS total_users,
        SUM(visited_landing_page)                                          AS visited_lp,
        SUM(started_trial)                                                 AS started_trial,
        SUM(converted_to_paid)                                             AS converted_to_paid,

        ROUND(SAFE_DIVIDE(SUM(started_trial), SUM(visited_landing_page))   * 100, 1) AS lp_to_trial_pct,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), SUM(started_trial))      * 100, 1) AS trial_to_paid_pct,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), SUM(visited_landing_page)) * 100, 1) AS overall_conversion_pct

    FROM funnel_stages
    GROUP BY channel
    ORDER BY overall_conversion_pct DESC
),


-- -----------------------------------------------------------------------------
-- CTE 7: conversion_by_segment_channel
-- Cross-segmentation: segment × channel matrix.
-- Identifies the highest-value combinations for acquisition targeting.
-- -----------------------------------------------------------------------------
conversion_by_segment_channel AS (
    SELECT
        segment,
        channel,
        COUNT(*)                                                             AS total_users,
        SUM(converted_to_paid)                                               AS converted_to_paid,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), COUNT(*)) * 100, 1)       AS overall_conversion_pct,

        -- Rank channels within each segment by conversion rate
        RANK() OVER (
            PARTITION BY segment
            ORDER BY SAFE_DIVIDE(SUM(converted_to_paid), COUNT(*)) DESC
        ) AS channel_rank_within_segment

    FROM funnel_stages
    GROUP BY segment, channel
),


-- -----------------------------------------------------------------------------
-- CTE 8: key_feature_impact
-- Analyzes the impact of using the key feature on final conversion.
-- Validates hypothesis: users who reach 'used_key_feature = 1' convert at higher rates.
-- -----------------------------------------------------------------------------
key_feature_impact AS (
    SELECT
        used_key_feature,
        COUNT(*)                                                             AS users,
        SUM(converted_to_paid)                                               AS converted,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), COUNT(*)) * 100, 1)       AS conversion_rate_pct
    FROM funnel_stages
    WHERE started_trial = 1  -- only users who entered the trial (fair comparison)
    GROUP BY used_key_feature
),


-- -----------------------------------------------------------------------------
-- CTE 9: plan_conversion
-- Conversion rates broken down by pricing plan (Basic, Pro, Business).
-- Supports hypothesis on plan-segmented nurturing sequences.
-- -----------------------------------------------------------------------------
plan_conversion AS (
    SELECT
        plan,
        COUNT(*)                                                           AS total_users,
        SUM(started_trial)                                                 AS started_trial,
        SUM(converted_to_paid)                                             AS converted_to_paid,
        ROUND(SAFE_DIVIDE(SUM(started_trial), COUNT(*))        * 100, 1)  AS trial_start_rate_pct,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), COUNT(*))    * 100, 1)  AS overall_conversion_pct,
        ROUND(SAFE_DIVIDE(SUM(converted_to_paid), SUM(started_trial)) * 100, 1) AS trial_to_paid_pct
    FROM funnel_stages
    GROUP BY plan
    ORDER BY overall_conversion_pct DESC
)


-- =============================================================================
-- FINAL OUTPUT: Full funnel summary
-- Combine conversion_rates with segment and channel breakdowns.
-- In production, each CTE above would be materialized as a separate view or
-- referenced independently by the BI layer (Looker Studio, Power BI, etc.).
-- =============================================================================

-- Uncomment the SELECT below to run a specific analysis:

-- Overall funnel rates:
SELECT * FROM conversion_rates;

-- By segment:
-- SELECT * FROM conversion_by_segment;

-- By channel:
-- SELECT * FROM conversion_by_channel;

-- Segment × channel matrix (top combos only):
-- SELECT * FROM conversion_by_segment_channel WHERE channel_rank_within_segment = 1;

-- Key feature impact on conversion:
-- SELECT * FROM key_feature_impact;

-- Plan-level conversion:
-- SELECT * FROM plan_conversion;
