# GRDS OverView

#### Introduction

GRDS\(Global Risk Data & Science\) a big department in paypal which comes from a startup company in Israel. They use many different math models and analysis algorithms to check millions of business transactions everyday in the world.

#### Important parts

GRDS contains many services, platforms and data infra.  
One data infra called **IDI Analytics** System Infra.  
Three of the services are **IRAS \(IdiRiskAccessService - stable one\)**, **RTCS \(RiskUnifiedComputeService - not stable\)**, **RUCS\(not stable\)**, they are running in IDI. IRAS will merge into RTCS and RUCS in future.

### I. IRAS Overview

#### What is IRAS?

IRAS is a read-only system. It reads variables from data set or other sources, but will not write anything to the database. This service use different variables\(location, IP address, time,etc\) as the input, calculate them through it's models, and output some variable. In fact, these output results will be used by higher level applications.

#### Input Data Type

1. **On the flat\(OTF\)**: It is some basic data attached with the request. Since the amount of data is small, we can get the output result in a short time.\(ms\)
2. **RADD**: It is some history data\(All the transaction records of one people in the last three years\). Since it contains a large amount of data, it needs a long time to get the result. It is not realtime result.
3. **Edge**: Edge is a type of data which between OTF and RADD. It has better realtime performance compare with RADD

#### Tools

1. **Simulation**: simulation is a tool which can be used to simulate live environment. We can deploy some jobs on it to test the new build.
2. **MRT**: mrt is a compare tool. When we need to do some regression test, we have to compare some variables output by the candidate build and live build.

#### Release

1. **ISO release**: IRAS analytics build will release every two weeks\(usually\). It contains a lot of changes, like model change \(model strategy change, intro some new models\), variable change\(intro some new variables\). It needs a formal test: simulation test, stage test, fqa\(functional quality assurance\).  

   When new fraud action was detected, data scientists have to collect the fraud information and update their model. All the updates need to be integrated into the new release.

2. **Non-ISO release**: When some small model changes were made\(except add new variable\), they will edit the model configuration file, no need to waiting for the formal release. For this kind of release, we don't need to do the formal test. Simulation controller test is enough.

#### CheckPoints

IRAS have 3 workflows which correspond to 3 checkpoints:

1. **BRE\(transaction\)**: It is the most important part and needs short response time\(&lt;=100ms\).But we don't have much work at this time. It is a realtime action\(sync\). Already transfer to RTCS, RUCS
2. **Withdraw**: Many changes were made in this part within this period, it will consume most of your work time. It is realtime action also.
3. **POS**: Less important. 

#### Online & Offline

1. **Online**: Online means a realtime system, users of this system want to get response in a short time\(&lt;=100ms to guarantee a good user experience\). For example, user login. It doesn't need to calculate a lot of data, but it is time sensitive which means we have heavy work to do to support it in time.
2. **Offline**: Offline means not a realtime system. We don't need reponse in a short time. But it contains a larger amount of data which needs many algorithms and models. The analysis process takes a long time and consumes many resources.

