---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

# What is Load Testing?

Load testing is a subset of performance testing that looks for how a system responds to normal and peak usage.

We can check slow response times, errors, crashes, and other issues to determine how many users and transactions the
system can accommodate before performance suffers.

# Load Testing vs Performance Testing

While load testing and performance testing are related, they are distinct types of testing.

| Testing             | Description                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Load testing        | Load testing simulates user activity to determine how well a system can handle increased traffic or load.                                                                                                                                                                                                                                                                                                                 |
| Performance testing | **Performance testing is an umbrella term for measuring how well a system or application performs overall.** This could include testing for speed, scalability, reliability, and resource utilization in order to identify areas of improvements. <br/><br/> **And performance testing includes load testing but also encompasses other types of testing**, such as browser performance testing and synthetic monitoring. |

# Types of Load Testing

| Type                                                                | VUs/Throughput                 | Duration                   | When?                                                                                                                                                       |
|---------------------------------------------------------------------|--------------------------------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Smoke                                                               | Low                            | Short (seconds or minutes) | For checking functional logic, basline metrics, and deviations.                                                                                             |
| Average-Load <br/> (Day-In-Life Test, Volume Test)                  | Average production load        | Medium (5-60mins)          | For checking system maintains performance with average use.                                                                                                 |
| Soak  <br/> (Endurance Test, Constant High Load Test, Stamina Test) | Average                        | Long(hours)                | For checking <br/> - degradation of performance and resource consumption over extended periods. <br/> - availability and stability during extended periods. |
| Stress                                                              | High (above average)           | Medium (5-60mins)          | For checking system behavior and how system works when the system receives above average loads.                                                             |
| Spike                                                               | Very High                      | Short (a few minutes)      | For checking system behavior and how system works when the system receives  traffic peaks(like seasonal events).                                            |
| Breakpoint                                                          | Increases until break(failure) | As long as necessary       | For checking the system's limits.                                                                                                                           |
|                                                                     |                                |                            |                                                                                                                                                             |

# References

https://grafana.com/load-testing/types-of-load-testing/