---
title: "Simplifying AWS defense with Microsoft Sentinel UEBA"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/28/simplifying-aws-defense-microsoft-sentinel-ueba/"
date: "Tue, 28 Apr 2026 13:00:00 +0000"
author: "Microsoft Defender Security Research Team"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<aside class="table-of-contents-block accordion wp-block-bloginabox-theme-table-of-contents" id="accordion-655cc34e-ee8a-4966-a125-67ed13042bfe">
	<button class="btn btn-collapse" type="button">
		<span class="table-of-contents-block__label">In this article</span>
		<span class="table-of-contents-block__current"></span>

		<svg class="table-of-contents-block__arrow" fill="none" height="11" viewBox="0 0 18 11" width="18" xmlns="http://www.w3.org/2000/svg">
			<path d="M15.7761 11L18 8.82043L9 0L0 8.82043L2.22394 11L9 4.35913L15.7761 11Z" fill="currentColor">
		</svg>
	</button>
	<div class="table-of-contents-block__collapse-wrapper collapse show" id="accordion-collapse-655cc34e-ee8a-4966-a125-67ed13042bfe">
		<div class="table-of-contents-block__content">
			<ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#under-the-hood-the-tables">Under the hood: The tables</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#traditional-vs-new-approach">Traditional vs. new approach</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#real-world-attack-scenarios-microsoft-sentinel-ueba-in-action">Real-world attack scenarios: Microsoft Sentinel UEBA in action</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#practical-implementation-getting-started">Practical implementation: Getting started</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#limitations-and-constraints">Limitations and constraints</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#from-raw-logs-to-behavioral-context">From raw logs to behavioral context</a></li></ol>		</div>
	</div>
	<span class="table-of-contents-block__progress-bar"></span>
</aside>



<p class="wp-block-paragraph">With the <a href="https://techcommunity.microsoft.com/blog/microsoftsentinelblog/microsoft-sentinel%E2%80%99s-ai-driven-ueba-ushers-in-the-next-era-of-behavioral-analyti/4448390">expansion of Microsoft Sentinel UEBA (User and Entity Behavior Analytics) into new data sources</a>, spanning multi-cloud (AWS, GCP), identity providers (Okta), and authentication logs (Microsoft Defender for Endpoint DeviceLogon, Microsoft Entra ID Managed Identity, Service Principal sign-ins), defenders can now detect behavioral anomalies across hybrid environments from a single place.</p>



<p class="wp-block-paragraph">We’ve also expanded AWS coverage with more anomalies, enrichments and insights, so CloudTrail events now arrive with more built-in context at ingestion time. This lets defenders triage suspicious activity faster without building and maintaining large baselines in KQL.</p>



<p class="wp-block-paragraph">Many defenders analyze CloudTrail activity using thresholds or historical patterns to identify unusual behavior. In dynamic cloud environments, interpreting this activity can be challenging without additional behavioral context.</p>



<p class="wp-block-paragraph"><a href="https://learn.microsoft.com/en-us/azure/sentinel/enable-entity-behavior-analytics?tabs=azure">Microsoft Sentinel UEBA</a> shifts the burden away from query authors by enriching raw AWS logs with simple binary insights (true/false) derived from user, activity, and device behavior patterns – such as first-time geography, uncommon ISP, unusual action, and abnormal operation volume. Detection authors can stack these binary signals or combine them with built-in UEBA anomalies to surface attacker behavior that would otherwise blend into routine CloudTrail activity.</p>



<p class="wp-block-paragraph">In this post, you’ll learn how binary feature stacking works, how UEBA baselines AWS identities (human and non-human), and how to use UEBA enrichments and built-in anomalies to strengthen AWS detections and triage.</p>



<p class="wp-block-paragraph">Defenders investigating AWS activity often rely on raw CloudTrail logs, static thresholds, or manually-engineered baselines to differentiate between normal operational patterns and adversary behavior. While CloudTrail captures rich activity data, defenders often need behavioral context – such as historical usage patterns, geography, and device signals – to distinguish routine operations from suspicious behavior. This is where Microsoft Sentinel UEBA adds value.</p>



<p class="wp-block-paragraph">Microsoft Sentinel UEBA enriches raw AWS logs with simple, binary behavioral insights (true/false) derived from baseline user, peer, and device behavior patterns – such as first-time geography, uncommon ISP, unusual action, and abnormal operation volume. These clear binary signals help establish behavioral context and inform investigation and detection decisions. This post refers to this approach as binary feature stacking.<a id="_msocom_1"></a></p>



<h2 class="wp-block-heading" id="under-the-hood-the-tables">Under the hood: The tables</h2>



<p class="wp-block-paragraph">Microsoft Sentinel UEBA surfaces AWS behavioral context in two tables: BehaviorAnalytics and Anomalies.</p>



<h3 class="wp-block-heading" id="behavioranalytics-table">BehaviorAnalytics table</h3>



<p class="wp-block-paragraph">The BehaviorAnalytics table is the primary investigation surface for UEBA-enriched AWS activity. EventSource field identifies the log source (for example, AWSCloudTrail), ActivityType maps to service level AWS <em>EventSource</em> (for example, S3, KMS, or IAM), and ActionType captures the AWS API name (for example, ConsoleLogin or CreateUser). Use these fields to filter and scope specific categories of AWS activity.<a id="_msocom_4"></a></p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146854 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-58.webp" /><figcaption class="wp-element-caption">Figure 1: BehaviorAnalytics table schema.</figcaption></figure>



<p class="wp-block-paragraph">UEBA provides enrichments in three dynamic fields&nbsp;(UserInsights, DeviceInsights and ActivityInsights)– most importantly ActivityInsights, a JSON property bag that contains the binary behavioral features used for baseline-driven profiling. These enrichments are calculated at the user and tenant (AWS AccountId) level, as well as the activity level (for example, uncommon high volume of operations). Each enrichment uses a different baseline window, ranging from 7 days to 180 days.</p>



<p class="wp-block-paragraph">This data is always available for hunting, even if no alert is fired. Each record includes key fields from the original CloudTrail event alongside enrichments derived from user,&nbsp;activity, and device behavior. The full list of available enrichments and their baseline lookback periods is documented in <a href="https://learn.microsoft.com/en-us/azure/sentinel/ueba-reference?tabs=log-analytics#entity-enrichments-dynamic-fields"><strong>Entity enrichments – dynamic fields</strong></a>.</p>



<h3 class="wp-block-heading" id="anomalies-table"><strong>Anomalies table</strong><a id="_msocom_1"></a></h3>



<p class="wp-block-paragraph">The Anomalies table contains outputs from Microsoft’s pre-trained anomaly detection machine learning models. Six built-in anomalies are currently available for AWS. For more information about these anomalies, see: <a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalies">Anomalies detected by the Microsoft Sentinel machine learning engine</a>.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146855 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-59.webp" /><figcaption class="wp-element-caption">Figure 2: Anomalies table schema.</figcaption></figure>



<p class="wp-block-paragraph">Each anomaly record includes MITRE ATT&amp;CK mappings, behavioral enrichments, an AnomalyScore, and AnomalyReasons<em>,</em> which explains why an event was flagged as an anomaly.</p>



<p class="wp-block-paragraph">Here’s an example of an AWS IAM Privilege Modification anomaly. In this case, the <em>CreateLoginProfile</em> API was invoked from a previously unseen user agent in a new country. The annotated screenshot illustrates how the anomaly is displayed and how the AnomalyReasons dynamic field provides binary insights that help investigation. In addition to FirstTimeUserPerformedAction and FirstTimeUserConnectedFromCountry, the BrowserUncommonlyUsedInTenant feature indicates a new user agent <em>(Apache-HttpClient/UNAVAILABLE (Java/21.0.9))</em> not commonly seen in the tenant.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146856 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-60.webp" /><figcaption class="wp-element-caption">Figure 3: AWS IAM Privilege Modification anomaly.</figcaption></figure>



<p class="wp-block-paragraph">The Defender portal also surfaces UEBA anomalies on user entity pages and incident graphs.</p>



<p class="wp-block-paragraph">This example highlights the Top UEBA anomalies section in an incident graph, where an <em>Anomalous Logon</em> event is displayed with an anomaly score of <em>0.8 </em>for the account entity <em>cloudinfra-admin.</em></p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146857 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-61.webp" /><figcaption class="wp-element-caption">Figure 4: Top UEBA anomalies on an incident graph in the Defender portal,</figcaption></figure>



<p class="wp-block-paragraph">You can run built-in queries directly from incident graphs by selecting Go Hunt  All User anomalies queries for immediate context-driven hunting based on UEBA outcomes. For more details, see <a href="https://learn.microsoft.com/en-us/azure/sentinel/ueba-reference?tabs=log-analytics#ueba-integration-with-microsoft-sentinel-workflows">UEBA integration with Microsoft Sentinel workflows</a>.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146858 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-62.webp" /><figcaption class="wp-element-caption">Figure 5: Hunt for all user anomalies from an incident graph in the Defender portal.</figcaption></figure>



<h2 class="wp-block-heading" id="traditional-vs-new-approach">Traditional vs. new approach</h2>



<p class="wp-block-paragraph">Let’s look at a classic AWS scenario: Unusual anomalous AWS logons. You want to detect a user&#8217;s sign in from an unknown location compared to its historical sign-in patterns.</p>



<h3 class="wp-block-heading" id="the-hard-way-raw-log-analytics">The hard way: Raw log analytics</h3>



<p class="wp-block-paragraph">CloudTrail telemetry can be analyzed using historical baselines and enrichment logic to understand behavioral patterns such as first‑time sign‑ins from new locations. UEBA complements this approach by providing pre‑computed behavioral indicators that can accelerate investigation workflows.</p>



<p class="wp-block-paragraph">Here is the KQL example on raw log showing necessary operations to add behavioral context.</p>



<p class="wp-block-paragraph">KQL Code Snippet:<a id="_msocom_1"></a></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
// The "Hard Way" - baseline-heavy console sign-in analytics
let baselineStart = ago(14d);
let baselineEnd   = ago(1h);
let userBaseline =    AWSCloudTrail
    | where TimeGenerated between (baselineStart .. baselineEnd)
    | where EventName == "ConsoleLogin" and isempty(ErrorCode)
    | where isnotempty(SourceIpAddress)
    | extend geo = geo_info_from_ip_address(SourceIpAddress)
    | extend Country = tostring(geo["country"])
    | where isnotempty(Country)
    | summarize HistoricalCountries = make_set(Country) by UserIdentityPrincipalid;
AWSCloudTrail
| where TimeGenerated > ago(1h)
| where EventName == "ConsoleLogin" and isempty(ErrorCode)
| where isnotempty(SourceIpAddress)
| extend geo = geo_info_from_ip_address(SourceIpAddress)
| extend Country = tostring(geo["country"])
| where isnotempty(Country)
| join kind=leftouter (userBaseline) on UserIdentityPrincipalid
| extend FirstTimeUserConnectedFromCountry =    iif(isempty(HistoricalCountries) or not(set_has_element(HistoricalCountries, Country)), true, false)
| where FirstTimeUserConnectedFromCountry == true
</pre></div>


<p class="wp-block-paragraph">The problem: This query is computationally expensive, hard to read, and requires you to manually enrich IP addresses with location data. Accurately mapping IP addresses to ASN and ISP often requires additional enrichments and up to date lookup databases. Because different user behaviors have different levels of variability, static thresholds and manually engineered baselines can still produce false positives or low-value alerts, especially in dynamic environments.</p>



<h3 class="wp-block-heading" id="the-smart-way-binary-feature-stacking">The smart way: Binary feature stacking</h3>



<p class="wp-block-paragraph">With Microsoft Sentinel UEBA, the profiling engine has already done the heavy lifting. It learns the user&#8217;s sign-in patterns, peer commonality, and tenant-wide behavioral patterns, and outputs the result to the BehaviorAnalytics table as a set of pre-calculated binary features (true/false flags).</p>



<p class="wp-block-paragraph">KQL Code Snippet:</p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
// The "Smart Way" - leveraging binary features
BehaviorAnalytics
| where ActionType == "ConsoleLogin" and ActivityType == "signin.amazonaws.com" 
// The Binary Features
| where ActivityInsights.FirstTimeUserConnectedFromCountry == True
and ActivityInsights.CountryUncommonlyConnectedFromInTenant == True and ActivityInsights.FirstTimeConnectionViaISPInTenant == True
</pre></div>


<p class="wp-block-paragraph"><strong>The advantages:</strong></p>



<ol class="wp-block-list">
<li class="wp-block-list-item"><strong>Readability:</strong> It takes just three lines of code to express a complex idea with UEBA features.</li>



<li class="wp-block-list-item"><strong>Context:</strong> You’re not just looking at uncommon <strong>sign ins</strong>. You’re stacking user-level and tenant-level indicators – such as location data (FirstTimeUserConnectedFromCountry) and uncommon ISP usage (FirstTimeConnectionViaISPInTenant) – to get a more accurate representation of suspicious behaviors relative to historical patterns.</li>



<li class="wp-block-list-item"><strong>Stability:</strong> You don&#8217;t manage the baseline, lookback, and thresholds in your query. The Microsoft Sentinel UEBA ML engine maintains these automatically with baseline windows that vary by enrichment (ranging from 7 to 180 days).</li>
</ol>



<p class="wp-block-paragraph">By relying on these binary features, detection authors stop writing code to <em>discover</em> behavioral signals and instead use UEBA features to express detection intent and how to <em>respond</em> based on severity.</p>



<p class="wp-block-paragraph">Now let’s look at how these same signals appear during investigation and triage.</p>



<h2 class="wp-block-heading" id="real-world-attack-scenarios-microsoft-sentinel-ueba-in-action">Real-world attack scenarios: Microsoft Sentinel UEBA in action<a id="_msocom_1"></a></h2>



<p class="wp-block-paragraph">The table below summarizes four attack scenarios using a consistent set of fields:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><strong>Scenario</strong>: The threat pattern and where it fits in the kill chain.</li>



<li class="wp-block-list-item"><strong>The attack</strong>: What the adversary is attempting to do&nbsp;(high-level behavior).</li>



<li class="wp-block-list-item"><strong>Common log view</strong>: How the activity appears in raw CloudTrail when reviewed event-by-event.</li>



<li class="wp-block-list-item"><strong>UEBA signals (binary features)</strong>: BehaviorAnalytics binary features that provide behavioral context, along with the InvestigationPriority score (0-10) used to tune the severity of deviations.</li>



<li class="wp-block-list-item"><strong>Built-in anomaly surfaced</strong>: Names of built-in Microsoft Sentinel UEBA anomalies you can pivot to during triage, including AnomalyScore (0–1) and AnomalyReasons in the Anomalies table.</li>
</ul>



<p class="wp-block-paragraph">Together, these scenarios illustrate how raw CloudTrail events provide foundational visibility into AWS activity. Combining this telemetry with behavioral enrichment from Sentinel UEBA can improve the interpretability of events during investigation. The same building blocks—successful sign-ins, IAM changes, Secrets or KMS access, and S3 reads—can represent either normal administration or active intrusion.</p>



<p class="wp-block-paragraph">By combining CloudTrail activity with Sentinel UEBA enrichments in BehaviorAnalytics, defenders can stack multiple high-value signals to hunt for activity patterns that match attacker tradecraft.</p>



<p class="wp-block-paragraph">This context accelerates investigations by making it easier to explain <em>why</em> an action is suspicious and to pivot directly to&nbsp;correlated entries in the Anomalies table, including risk scores and reasons. For detection engineers, UEBA signals also act as stable building blocks—simplifying KQL, reducing alert noise, and hardening detections over time.</p>



<p class="wp-block-paragraph">Note: The UEBA signals column lists examples of relevant binary features, not the exact logic that triggers an anomaly. Anomalies are generated by ML models and don’t map one-to-one to individual features. Use AnomalyReasons in the Anomalies table to understand why a specific anomaly was flagged.</p>



<h3 class="wp-block-heading" id="attack-scenarios">Attack scenarios</h3>



<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Scenario</strong></td><td><strong>The attack</strong></td><td><strong>Common log view</strong></td><td><strong>UEBA signals (binary features)</strong></td><td><strong>Built-in anomaly surfaced</strong></td></tr><tr><td><strong>Initial Access (Federated / SAML Session Hijack)</strong></td><td>An attacker gains access to a federated identity session – for example, through a compromised identity provider (IdP) – and uses a SAML or EXTERNAL_IDP flow to perform actions the user rarely performs, from a new location and at an unusual pace.</td><td>CloudTrail shows federated authentication activity (UserAuthentication / EXTERNAL_IDP, for example, Okta) followed by successful API calls under an assumed role session; each event is valid in isolation.</td><td>FirstTimeUserConnectedFromCountry = <strong>True</strong><br /><br />ISPUncommonlyUsedInTenant = <strong>True</strong><br /><br />ActionUncommonlyPerformedByUser = <strong>True</strong><br /><br />ActionUncommonlyPerformedInTenant = <strong>True</strong></td><td>UEBA Anomalous Federated or SAML Identity Activity in AwsCloudTrail</td></tr><tr><td><strong>Initial Access and Persistence</strong></td><td>An attacker compromises a developer’s access keys and logs in (for example, through uncommon user agent) to create a backdoor user.</td><td>CloudTrail shows a successful ConsoleLogin via SDK or CLI user agent and subsequent IAM action, such as CreateUser, all of which are valid API calls without behavioral context.</td><td>FirstTimeUserConnectedFromCountry = <strong>True</strong><br /><br />BrowserUncommonlyUsedInTenant<strong> = True</strong><br /><br />ActionUncommonlyPerformedByUser = <strong>True</strong> (CreateUser)<br /><br />ActionUncommonlyPerformedInTenant = <strong>True</strong></td><td>Examples: UEBA Anomalous Logon in AwsCloudTrail; UEBA Anomalous IAM Privilege Modification in AwsCloudTrail</td></tr><tr><td><strong>Credential Access &amp; Collection (Secrets / KMS Key Discovery)</strong></td><td>After establishing a foothold with valid credentials, an attacker queries Secrets Manager and KMS to list keys and retrieve secret values, often starting with discovery (ListSecrets/ListKeys) then access (GetSecretValue), sometimes at unusually high frequency.</td><td>CloudTrail shows a GetSecretValue, ListSecrets, or ListKeys activity which can look like legitimate automation and make static allowlists and thresholds brittle.</td><td>FirstTimeUserPerformedAction = <strong>True</strong><br /><br />ActionUncommonlyPerformedInTenant = <strong>True</strong><br /><br />UncommonHighVolumeOfOperations = <strong>True</strong><br /><br />ISPUncommonlyUsedInTenant = <strong>True</strong></td><td>UEBA Anomalous Secret or KMS Key Access in AwsCloudTrail</td></tr><tr><td><strong>Data Exfiltration (the “low-and-slow” S3 drain)</strong></td><td>A compromised admin account performs a burst of repeated <strong>S3 GetObject</strong> operations—representing a high volume of similar operations within the same service—often targeting multiple objects or prefixes in quick succession to stage data for exfiltration while staying below traditional volume thresholds.</td><td>If S3 data events are enabled, CloudTrail shows a high frequency of <strong>GetObject</strong> API calls across multiple objects or buckets in a short time window. Each request appears legitimate in isolation, and overall data transfer may remain below static thresholds, making the activity difficult to detect using traditional methods.</td><td>UncommonHighVolumeOfOperations = <strong>True</strong><br /><br />CountryUncommonlyPerformedInTenant = <strong>True</strong><br /><br />ActionUncommonlyPerformedByUser = <strong>True</strong> (S3 GetObject)<br /><br />ISPUncommonlyUsedInTenant = <strong>True</strong></td><td>UEBA Anomalous Data Transfer from Amazon S3</td></tr></tbody></table></figure>



<p class="wp-block-paragraph"><em>Table 1: Examples of Microsoft Sentinel UEBA enrichments in real-world attack scenarios</em><a id="_msocom_1"></a></p>



<h3 class="wp-block-heading" id="built-in-microsoft-sentinel-ueba-anomaly-mitre-att-ck-coverage">Built-in Microsoft Sentinel UEBA anomaly MITRE ATT&amp;CK coverage</h3>



<p class="wp-block-paragraph">The visual below illustrates how Microsoft Sentinel UEBA’s AWS anomaly coverage maps across multiple stages of the kill chain:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Initial access: <a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalous-federated-or-saml-identity-activity-in-awscloudtrail">UEBA Anomalous Federated or SAML Identity Activity in AwsCloudTrail</a></li>



<li class="wp-block-list-item">Initial access/privilege escalation: <a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalous-sts-assumerole-behavior-in-awscloudtrail">UEBA Anomalous STS AssumeRole Behavior in AwsCloudTrail</a></li>



<li class="wp-block-list-item">Persistence/privilege escalation: <a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalous-iam-privilege-modification-in-awscloudtrail">UEBA Anomalous IAM Privilege Modification in AwsCloudTrail</a></li>



<li class="wp-block-list-item">Credential access/collection: <a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalous-secret-or-kms-key-access-in-awscloudtrail">UEBA Anomalous Secret or KMS Key Access in AwsCloudTrail</a></li>



<li class="wp-block-list-item">Collection/data exfiltration: <a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalous-data-transfer-from-amazon-s3">UEBA Anomalous Data Transfer from Amazon S3</a></li>
</ul>



<p class="wp-block-paragraph">Together, these anomaly detections provide defenders with end-to-end visibility – from suspicious authentication through sensitive access and data movement – with binary feature enrichments that add high-value behavioral context during investigations.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146859 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-63.webp" /><figcaption class="wp-element-caption">Figure 6: Microsoft Sentinel UEBA&rsquo;s AWS anomaly coverage across the attack chain.</figcaption></figure>



<h2 class="wp-block-heading" id="practical-implementation-getting-started">Practical implementation: Getting started</h2>



<p class="wp-block-paragraph"><strong>Before you begin</strong></p>



<p class="wp-block-paragraph">Before you run the queries, ensure the following are in place:</p>



<p class="wp-block-paragraph"><strong>Baseline establishment period completed</strong><br />Allow sufficient time for UEBA to establish user, activity, and device baselines. In most environments, this typically requires 7–14 days of steady telemetry.<a id="_msocom_1"></a></p>



<p class="wp-block-paragraph"><strong>AWS environment onboarded to Microsoft Sentinel UEBA</strong><br />Ensure that AWS CloudTrail (management events and, where applicable, object-level data) is connected, and UEBA is enabled for the AWS data source.</p>



<p class="wp-block-paragraph"><strong>CloudTrail data is flowing consistently</strong><br />Confirm that AWS CloudTrail events are being ingested into Microsoft Sentinel and visible in Advanced Hunting.</p>



<h4 class="wp-block-heading" id="starter-query"><strong>Starter query</strong></h4>



<p class="wp-block-paragraph">Ready to start hunting? Open Advanced Hunting in Microsoft Sentinel and run the following query to explore the BehaviorAnalytics table and inspect enriched AWS behavioral signals. This query intentionally keeps the logic lightweight. The goal is not to “detect” anomalous activity immediately, but to understand how binary behavioral features surface in your environment.<a id="_msocom_1"></a></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
// Starter query – explore UEBA-enriched AWS behavioral signals
BehaviorAnalytics
| where EventSource == "AWSCloudTrail" or ActivityType endswith "amazonaws.com"
| where isnotempty(ActivityInsights)
| where ActivityInsights.FirstTimeUserConnectedFromCountry == true
   or ActivityInsights.ActionUncommonlyPerformedByUser == true
   or ActivityInsights.UncommonHighVolumeOfOperations == true
| project
    TimeGenerated,
    UserName,
    ActionType,
    EventSource,
    ActivityType,
    ActivityInsights
| order by TimeGenerated desc
</pre></div>


<h4 class="wp-block-heading" id="what-to-look-for"><strong>What to look for</strong></h4>



<p class="wp-block-paragraph">When reviewing the results, focus on:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><strong>Binary feature combinations</strong><br />Individual binary indicators may be benign. Risk emerges when multiple features align (for example: first-time geography <em>and</em> uncommon action).</li>



<li class="wp-block-list-item"><strong>User-centric deviations</strong><br />Pay attention to activity that is unusual <em>for that specific identity</em>, even if the action itself is common across the <strong>tenant</strong>.</li>



<li class="wp-block-list-item"><strong>Low-volume but persistent activity</strong><br />UEBA often highlights slow, methodical behavior (for example, repeated S3 reads or Secrets/KMS access) that <strong>stays below static thresholds but persists over time</strong>.</li>



<li class="wp-block-list-item"><strong>Candidates for anomaly pivoting</strong><br />Events that exhibit multiple binary features warrant further investigation by pivoting to the <strong>Anomalies</strong> table, where UEBA may have already produced a correlated anomaly record with supporting context and reasoning.</li>
</ul>



<h4 class="wp-block-heading" id="common-false-positives-and-how-to-filter-them"><strong>Common false positives (and how to filter them)</strong></h4>



<ul class="wp-block-list">
<li class="wp-block-list-item"><strong>Legitimate automation or CI/CD pipelines</strong><br /><em>Why it happens:</em> Service roles or automation accounts may perform actions infrequently or from new infrastructure locations.<br /><em>How to filter:</em> Exclude known accounts or IAM roles used exclusively for automation once validated. Be sure to filter only specific types of activities, rather than applying blanket exclusions.</li>



<li class="wp-block-list-item"><strong>New administrators or role changes</strong><br /><em>Why it happens:</em> First‑time admin activity naturally triggers “first‑time” and “uncommon” indicators depending on the baseline.<br /><em>How to filter:</em> Correlate with recent user creation or role assignment changes before suppressing.</li>



<li class="wp-block-list-item"><strong>Planned operational changes</strong><br /><em>Why it happens:</em> Migrations, incident response, or large‑scale maintenance can temporarily skew baselines.<br /><em>How to filter:</em> Use time‑bounded filters or change‑window context rather than permanently suppressing signals.</li>
</ul>



<h4 class="wp-block-heading" id="next-steps"><strong>Next steps</strong></h4>



<p class="wp-block-paragraph">Once you are comfortable interpreting enriched behavior:</p>



<ol class="wp-block-list" start="1">
<li class="wp-block-list-item">Stack binary features intentionally (especially User and Tenant level) in detection logic rather than alerting on single indicators.</li>



<li class="wp-block-list-item">Pivot to UEBA anomalies to leverage Microsoft’s pre-trained models across MITRE ATT&amp;CK tactics.</li>



<li class="wp-block-list-item">Promote successful hunts into detections with minimal additional KQL, relying on UEBA to maintain baselines over time.</li>
</ol>



<p class="wp-block-paragraph">This approach lets detection authors focus on behavioral intent, not baseline math – turning raw AWS telemetry into actionable security signals.<a id="_msocom_1"></a></p>



<h2 class="wp-block-heading" id="limitations-and-constraints">Limitations and constraints</h2>



<p class="wp-block-paragraph">Microsoft Sentinel UEBA can substantially reduce detection complexity, but it’s important to account for coverage boundaries and the conditions under which enrichments and scores are most reliable:</p>



<p class="wp-block-paragraph"><strong>Coverage is selective (not “every API”).</strong></p>



<ul class="wp-block-list">
<li class="wp-block-list-item">UEBA does not ingest or model every API call for every AWS service. CloudTrail can be extremely high-volume, so the Microsoft Sentinel UEBA pipeline focuses on the event sources and API actions that are most useful for behavior modeling and that are most commonly correlated with anomalous activity (for example, authentication, identity and permission changes, sensitive data access, and high-impact operations). You can always check the up-to-date list of in-scope event sources, APIs, and data sources in the <a href="https://learn.microsoft.com/en-us/azure/sentinel/ueba-reference?tabs=log-analytics#ueba-data-sources"><strong>UEBA data sources</strong></a> reference document (GCPAuditLogs, Okta log sources are also supported). We’re continually adding APIs and event sources.</li>
</ul>



<p class="wp-block-paragraph"><strong>Enrichments vary by event type.</strong></p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Not all enrichments are populated for all actions. For example, UncommonHighVolumeOfOperations is unlikely to apply to specific types of rare APIs, and location/ISP-related enrichments typically require the original source event to include a valid IP address.</li>
</ul>



<p class="wp-block-paragraph"><strong>Cross-cloud identity baselines are calculated independently</strong>.</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">UEBA profiles identities per data source, which means behavior across cloud platforms is baselined separately. Correlation across environments can be performed manually using the BehaviorAnalytics table when required.</li>
</ul>



<p class="wp-block-paragraph"><strong>Use scores for prioritization, not direct alerting without retroactive lookup.</strong></p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Treat the AnomalyScore (0-1) and InvestigationPriority (0-10) values as investigation signals to help rank what to look at first – not as sole triggers for alerts. The highest score may not always be the highest priority investigation for your organization. Validate patterns in your environment and use a combination of enrichments, scores, and repeat behavior over time before finalizing alert logic.</li>
</ul>



<p class="wp-block-paragraph"><strong>Anomaly support in the UI is currently for UPN-based entities.</strong></p>



<ul class="wp-block-list">
<li class="wp-block-list-item">AWS UEBA anomalies are currently surfaced in the UI only on the Account entity, which assumes an identity mapped to a UPN. This works well for environments that use Microsoft Entra ID (or another IdP) with UPN identifiers, but it might not apply to AWS IAM users or AWS resource entities that do not map cleanly to a UPN. To be clear – anomalies are triggered and available for all identity types (with UPN and without UPN), but are only shown in the UI for entities with a UPN.</li>
</ul>



<p class="wp-block-paragraph"><strong>Some insights depend on identity and user agent fidelity.</strong></p>



<ul class="wp-block-list">
<li class="wp-block-list-item">DeviceInsights rely on parsing UserAgent strings and may be unreliable if user agents are spoofed or manipulated in the original log. Some UserInsights enrichments also depend on identity inventory and metadata snapshots being available. Microsoft identity data from Microsoft Entra is synchronized automatically to the IdentityInfo table – other identity providers are not currently supported, so they might have more limited enrichment coverage.</li>
</ul>



<h2 class="wp-block-heading" id="from-raw-logs-to-behavioral-context">From raw logs to behavioral context</h2>



<p class="wp-block-paragraph">CloudTrail provides detailed activity data. Sentinel UEBA enhances this telemetry with behavioral context, such as first‑time geography or uncommon ISP usage, to support investigation and detection workflows. A single <em>failed console login</em> is often low signal on its own. That same event becomes far more meaningful when it’s paired with behavioral context, such as a first-time country, an unusual ISP, or activity on a rarely used admin account.</p>



<p class="wp-block-paragraph">By shifting our focus from writing complex queries to leveraging Microsoft Sentinel UEBA’s binary feature stacking, we gain three practical advantages:</p>



<ol class="wp-block-list">
<li class="wp-block-list-item"><strong>Efficiency:</strong> We replace baseline-heavy, maintenance-prone queries with simpler, more readable logic.</li>



<li class="wp-block-list-item"><strong>Accuracy:</strong> We reduce false positives and better tune severity by requiring multiple binary features to align before alerting.</li>



<li class="wp-block-list-item"><strong>Visibility:</strong> We uncover the <em>low-and-slow</em> attacks that static thresholds often miss.</li>
</ol>



<p class="wp-block-paragraph">For the modern SOC, the goal is not only to collect logs—it’s to understand behavior. Use the BehaviorAnalytics table as your starting point to understand what “normal” looks like in your environment, then pivot to related Anomalies when you need model-driven prioritization. In practice, this shifts investigations from “<em>What happened?</em>” to “<em>Is this consistent with expected behavior?</em>”</p>



<p class="wp-block-paragraph">Ready to start hunting? <a href="https://learn.microsoft.com/en-us/azure/sentinel/enable-entity-behavior-analytics?tabs=azure#enable-ueba-from-workspace-settings">Onboard your AWS environment to Microsoft Sentinel UEBA</a>, open Advanced Hunting, and run the starter query in the <em>Practical implementation</em> section to explore the BehaviorAnalytics and Anomalies tables in your environment.</p>



<h3 class="wp-block-heading" id="references">References</h3>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://learn.microsoft.com/azure/sentinel/enable-entity-behavior-analytics" rel="noreferrer noopener" target="_blank">UEBA onboarding and setting documentation</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/azure/sentinel/identify-threats-with-entity-behavior-analytics" rel="noreferrer noopener" target="_blank">Identify threats using UEBA</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/azure/sentinel/ueba-reference" rel="noreferrer noopener" target="_blank">UEBA enrichments and insights reference</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/azure/sentinel/anomalies-reference#ueba-anomalies">Anomalies detected by the Microsoft Sentinel machine learning engine</a></li>
</ul>



<h3 class="wp-block-heading" id="learn-more"><strong>Learn more</strong></h3>



<p class="wp-block-paragraph">Learn about the <a href="https://techcommunity.microsoft.com/blog/microsoftsentinelblog/turn-complexity-into-clarity-introducing-the-new-ueba-behaviors-layer-in-microso/4484493">UEBA Behaviors Layer</a> for AWS CloudTrail and other data sources.<a id="_msocom_1"></a></p>



<p class="wp-block-paragraph">The <a href="https://techcommunity.microsoft.com/blog/microsoftsentinelblog/ueba-solution-power-boost-practical-tools-for-anomaly-detection/4488277">Microsoft Sentinel UEBA Essentials solution</a> provides additional built-in queries.</p>



<p class="wp-block-paragraph"></p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/28/simplifying-aws-defense-microsoft-sentinel-ueba/">Simplifying AWS defense with Microsoft Sentinel UEBA</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
