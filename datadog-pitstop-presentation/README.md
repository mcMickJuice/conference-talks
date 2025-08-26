# DataDog in Member Marketplace

## How we've customized DD with custom actions and span modifications

- user session tracking
  - [PR](https://github.com/shipt/segway-next/pull/13441)
  - [DataDog session query](https://us5.datadoghq.com/rum/sessions?query=%40type%3Asession%20%40application.id%3A258666b0-d235-46b0-85ba-2a5f36be52d8%20env%3Aproduction%20%40usr.isLoggedIn%3Atrue&agg_m=count&agg_m_source=base&agg_t=count&fromUser=false&from_ts=1756116327871&to_ts=1756202727871&live=true) against [shipt.com](http://shipt.com) users
  - [Example in CMS](https://us5.datadoghq.com/dashboard/xft-vwp-eub/cms-fe-user-activity?fromUser=false&refresh_mode=sliding&from_ts=1753524380114&to_ts=1756202780114&live=true) where we track by role
  - [DataDog Docs](https://docs.datadoghq.com/real_user_monitoring/browser/advanced_configuration/?tab=npm#user-session)
- feature flag tracking
  - [PR](https://github.com/shipt/segway-next/pull/13446) that integrates FF and Exp FE state with DataDog
  - [FF/Experiment page in DD](https://us5.datadoghq.com/rum/feature-flags?query=%40application.id%3A258666b0-d235-46b0-85ba-2a5f36be52d8%20%40session.type%3Auser&fromUser=false&from_ts=1756116490832&to_ts=1756202890832&live=true)
  - [Dashboard utilizing FF](https://us5.datadoghq.com/dashboard/gwk-s9q-eq6/cms-deferred-hydration-experiment?fromUser=false&refresh_mode=sliding&from_ts=1755598126408&to_ts=1756202926408&live=true)
  - [DataDog Docs](https://docs.datadoghq.com/real_user_monitoring/feature_flag_tracking/setup/?tab=browser#custom-feature-flag-management)
- custom actions
  - [PR to add modalOpen actio](https://github.com/shipt/segway-next/pull/13649)n
  - [DataDog session query](https://us5.datadoghq.com/rum/sessions?query=%40type%3Aaction%20%40application.id%3A258666b0-d235-46b0-85ba-2a5f36be52d8%20env%3Aproduction%20%40action.type%3Acustom%20%40action.name%3AopenModal&agg_m=count&agg_m_source=base&agg_q=%40context.type&agg_t=count&analyticsOptions=%5B%22bars%22%2C%22dog_classic%22%2Cnull%2Cnull%2C%22value%22%5D&cols=&fromUser=false&viz=sunburst&from_ts=1756117050036&to_ts=1756203450036&live=true) grouping openModal actions by modal type
  - [DataDog Docs](https://docs.datadoghq.com/real_user_monitoring/guide/send-rum-custom-actions/?tab=npm)
- span modifications
  - [PR to add gql operation name to resource](https://github.com/shipt/segway-next/pull/13538)
  - [DataDog session query against this operation name](https://us5.datadoghq.com/rum/sessions?query=%40type%3Aresource%20%40application.id%3A258666b0-d235-46b0-85ba-2a5f36be52d8%20env%3Aproduction&agg_m=count&agg_m_source=base&agg_q=%40context.operationName&agg_q_source=base&agg_t=count&analyticsOptions=%5B%22bars%22%2C%22dog_classic%22%2Cnull%2Cnull%2C%22value%22%5D&cols=&fromUser=false&top_n=10&top_o=top&viz=sunburst&x_missing=true&from_ts=1756116827870&to_ts=1756203227870&live=true)
  - [Dashboard utilizing this property](https://us5.datadoghq.com/dashboard/bbc-n8d-4h6/baseuser-query?fromUser=false&refresh_mode=sliding&from_ts=1753524909879&to_ts=1756203309879&live=true)
  - [DataDog Docs for beforeSend property](https://docs.datadoghq.com/real_user_monitoring/browser/advanced_configuration/?tab=npm#enrich-and-control-rum-data) in config

## How we've operationalized DD so far

- [Monitors](https://us5.datadoghq.com/monitors/manage?q=team%3Amember-marketplace)
  - [User Session anomaly detection](https://shipt.slack.com/archives/C014FFSAFNU/p1755886025241289)
    - Web Vital regressions
- [Dashboards](https://us5.datadoghq.com/dashboard/lists/manual/13456?p=1)
  - [Performance benchmarks](https://us5.datadoghq.com/dashboard/63j-9xj-tc5/member-marketplace-web-performance-overview?fromUser=false&refresh_mode=sliding&from_ts=1755599033508&to_ts=1756203833508&live=true)
- One-off queries
  - [Copy graph in Session explorer](https://us5.datadoghq.com/rum/sessions?query=%40type%3Aview%20%40application.id%3A258666b0-d235-46b0-85ba-2a5f36be52d8%20env%3Aproduction%20%40usr.hasPlacedMarketplaceOrder%3Afalse%20%40usr.isLoggedIn%3Atrue%20%40view.name%3AGLOBAL_HOMEPAGE&agg_m=count&agg_m_source=base&agg_t=count&cols=&fromUser=false&viz=timeseries&from_ts=1756117508969&to_ts=1756203908969&live=true), paste in Slack, get an image preview

## Resources for learning more about DD

- [DataDog Learning Center](https://learn.datadoghq.com/)
- Recommend paths for FE engineers:
  - [Core Skills](https://learn.datadoghq.com/bundles/core-skills-learning-path)
  - [Frontend Engineers](https://learn.datadoghq.com/bundles/frontend-engineer-learning-path)
