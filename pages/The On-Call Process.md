## Background

The purpose of this page is to act as an anchor for tips, tricks & solution found while responding to incidents. As such this page should act as an index for our collective knowledge.

Before Going On-Call
- Make sure you know how to log into customer environments, including GA environments using `saml2aws` and `ssm-connect, etc.`
- Ensure your team knows you're on-call because you might not be able to get to all of your tickets for the sprint.
- On the week before going on call, inform the team you will be on call in the upcoming week
- Sync up with the last on-call person to make sure you know which (if any) active incidents are going so you can take them over.
## While On-Call
- Monitor #beluga-dev-support-escalations or #beluga-envops-support-escalations (as applicable) and your phone (Pagerduty) for any incoming tickets
- This board is also useful to monitor overall state of tickets: [https://jira.pingidentity.com/secure/RapidBoard.jspa?rapidView=1139](https://jira.pingidentity.com/secure/RapidBoard.jspa?rapidView=1139)
- Triage and support the incoming tickets, following the [Triage Process](https://confluence.pingidentity.com/display/PDA/Triage+Process)
- Make the team aware if you will be out of reach for any period of time
- Keep your phone on and accessible at all times. (Always ready to respond to a Sev 1 immediately)
- Refer to the [Beluga SLA](https://www.pingidentity.com/en/legal/support-policy-addendum-pingone-as.html) for severity criteria and expected response times
- Sev 1 issue:
- Notify the secondary on-call immediately
- Ensure to record all events so that the information can be used at a later time in case an RCA is required
- The goal is to try and restore service as soon as possible. (Could potentially involve a rollback)
- INCOM, according to [procedure](https://confluence.pingidentity.com/display/PingCloud/P1AS+Incident+Handling%2C+Response%2C+and+Postmortem%2C+Incident+Command+INCOM):
- Should create a new Slack channel. (<YYYYMMDD>-<incident-description>) Ensure you are invited. If not, contact INCOM to get added.
- Should create the war room Zoom call and share to the incident Slack channel
- Jump on the associated Zoom call and actively engage with other attendees to troubleshoot and resolve the issue
- Identify the issue
- Refer to the troubleshooting guide: <link-coming-soon>
- Sev 2 Issue
- You don't need to page the secondary unless you are stuck
- There is more time to diagnose the root cause and capture information
- If INCOM is not involved, GSO is responsible for setting up a war room Zoom call. Contact the issue reporter to take care of this.
- Join the war room Zoom call when one is set up.
- Lower Severities
- No work is expected to be done on these over the weekend
## After Going On-Call
- Sync up with the next on-call person to make sure they know about any active incidents so they can take them over.
- Expect to provide any requested information related to the incident to INCOM in the event of a Sev 1 incident