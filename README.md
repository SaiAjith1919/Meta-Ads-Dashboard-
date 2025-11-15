# Meta-Ads-Dashboard

**PROJECT TITLE**
" Meta Ads Performance Daseboard"

**Short description**
This Power BI dashboard is a Meta (Facebook/Instagram) Ads performance dashboard built to monitor ad events (impressions, clicks, shares, comments, purchases), budget metrics, audience breakdowns and geographic performance.

**Tech stack**
🧭 Power BI Desktop — report canvas, visuals, pages and PBIX-style data model.
🔢 DAX (measures) — custom measures for Impressions, Clicks, CTR, Engagement Rate, Conversion Rate, Purchase Rate, Totals, Averages, and dynamic titles.
🗂️ Power BI Data Model — ad_events, ads, campaigns, users, calendar tables and a dynamic measure selector.
🗺️ Custom visuals — calendar visual and multiple map visuals (MapBySquillion, OSM-based visuals).

**Tables & Columns**
ad_events — event-level table (columns: event_id, ad_id, event_time / Events Date, event_type such as Impression/Click/Share/Comment/Purchase, event hour, week, time_of_day etc).
ads — ad metadata (ad_id, campaign_id, ad_type, target_gender, target_age_group, target_interests, etc).
campaigns — campaign-level data (campaign_id, campaign name, start_date, end_date, duration_days, total_budget).
users — audience or user-level attributes (user_id, user_gender, user_age, age_group, country, location, interests).
Select Dynamic Measure — helper table used for dynamic metric selection and dynamic titles.
Multiple calendar/date tables — CALENDER TABLE and two LocalDateTable_... tables used for time intelligence and the calendar visual.

**Relationships**
The model contains 6 relationships
-->	ad_events.timestamp -> LocalDateTable_03c6fa61-... .Date
-->	ad_events.ad_id -> ads.ad_id
-->	ads.campaign_id -> campaigns.campaign_id
-->	ad_events.user_id -> users.user_id
-->	CALENDER TABLE.Date -> LocalDateTable_4bd36c53-... .Date
--> ad_events.Events Date -> CALENDER TABLE.Date
These relationships implement a star-like schema where ad_events is the central fact table with lookups into ads, campaigns, and users, and multiple date tables are connected to ad events for time-based slicing.

**DAX measures and calculation**
_**Core event counts (ad_events table)**_
•	Impression
 	Impression = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Impression"))
•	Clicks
 	Clicks = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Click"))
•	Shares
 	Shares = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Share"))
•	Comments
 	Comments = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Comment"))
•	Purchase
 	Purchase = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Purchase"))
_**Aggregated and ratio metrics**_
•	Engagements
 	Engagements = [Clicks] + [Shares] + [Comments]
•	Click Through Rate
 	Click Through Rate = DIVIDE([Clicks], [Impression], 0)
•	Engagement Rate
 	Engagement Rate = DIVIDE([Engagements], [Impression], 0)
•	Conversion Rate
 	Conversion Rate = DIVIDE([Purchase], [Clicks], 0)
•	Purchase Rate
 	Purchase Rate = DIVIDE([Purchase], [Impression], 0)

**Visuals and layout**
1.	Overview KPI cards / tiles — likely show top-level metrics such as Impressions, Clicks, Click Through Rate (CTR), Engagements, Conversion Rate, Purchase/Revenue if present, Total Budget, and Avg. Budget per Campaign.
2.	Time-series charts (area/line) — weekly or daily trend visuals driven by timestamp/date tables using Impression, Clicks, Engagements, Purchase. Controlled by Weekly Title and Hour Title measures for dynamic headings.
3.	Hourly heatmap or line chart — Event Hour and time_of_day columns and Hour Title measure indicate an hourly analysis section (peak hour analysis for impressions/clicks).
4.	Geographical map visual — presence of several map custom visuals and a Map measure plus users.location / users.country columns indicate a map (choropleth/bubble map) showing distribution of impressions, clicks, and purchases across locations.
5.	Demographic breakdown visuals (bar/stacked charts) — user_gender, age_group, target_gender, target_age_group fields and Select Dynamic Measure table indicate visuals for gender and age-group splits. The Select Dynamic Measure table suggests a dynamic selector (what metric to show on demographic charts) e.g., show CTR by age, Engagement Rate by gender, or raw counts.
6.	Campaign / Ad level table or matrix — listing campaigns.name, campaigns.total_budget, metrics per campaign; allows sorting to find top performing campaigns/ads.
7.	Ad type / platform filters & visuals — ad_platform and ad_type columns referenced by measures and the Add Type measure; likely slicers or small multiples that compare Facebook vs Instagram vs other platforms.
8.	Custom calendar visual — included in custom visuals; may be used to highlight days with spikes in activity or provide a compact date heat calendar.
9.	Slicers / Filters — date slicer(s) (connected to one or more date tables), platform slicer, campaign / ad type, demographic slicers (gender, age group), and potentially an Ad search box.
10.	Tooltips & dynamic titles — multiple measures named * Title indicate dynamic titles updating according to selected filters.

**Interactivity & UX behaviour**
•	Cross-filtering across visuals follows model relationships: selecting an ad or campaign filters events, users and budgets.
•	Date filtering: multiple date tables are present; the report likely uses a single primary date slicer that maps to ad_events.timestamp via LocalDateTable_03c6fa... or CALENDER TABLE depending on the visual. Be careful: multiple date tables could cause confusion if different visuals are bound to different date tables.
•	Dynamic measure selection: a Select Dynamic Measure table indicates the presence of a slicer or dropdown allowing the user to switch the metric used in demographic charts (e.g., switch between Impressions, Clicks, Engagement Rate, Conversion Rate).
•	Map interactions: map visual(s) let users zoom and click on countries/regions to filter other visuals.
•	Drillthrough / details: not explicitly extracted from the template, but typical patterns include clicking a campaign to see ad-level breakdowns in the matrix/table.

**full list of measures**
•	ad_events.Impression = COUNTROWS(FILTER(ad_events, ad_events[event_type]="Impression"))
•	ad_events.Clicks = COUNTROWS(FILTER(ad_events, ad_events[event_type]="Click"))
•	ad_events.Shares = COUNTROWS(FILTER(ad_events, ad_events[event_type]="Share"))
•	ad_events.Comments = COUNTROWS(FILTER(ad_events, ad_events[event_type]="Comment"))
•	ad_events.Purchase = COUNTROWS(FILTER(ad_events, ad_events[event_type]="Purchase"))
•	ad_events.Engagements = [Clicks] + [Shares] + [Comments]
•	ad_events.Click Through Rate = DIVIDE([Clicks], [Impression], 0)
•	ad_events.Engagement Rate = DIVIDE([Engagements], [Impression], 0)
•	ad_events.Conversion Rate = DIVIDE([Purchase], [Clicks], 0)
•	ad_events.Purchase Rate = DIVIDE([Purchase], [Impression], 0)
•	ad_events.Total Budget (measure present — expression not included in the exported snippet)
•	ad_events.Avg. Budget per Campaign (measure present)
•	ad_events.Map
•	ad_events.Weekly Title
•	ad_events.Hour Title
•	ad_events.Add Type
•	Select Dynamic Measure.Gender Title
•	Select Dynamic Measure.Age Title

**Dashboard Visual**
Page of Instagram : https://github.com/SaiAjith1919/Meta-Ads-Dashboard-/blob/main/Meta%20Ads%20Dashboard%20(Instagram%20page).png
Page of Facebook  : https://github.com/SaiAjith1919/Meta-Ads-Dashboard-/blob/main/Meta%20Ads%20Dashboard%20(Facebook%20page).png
