---
title: "8 best practices for CISOs conducting risk reviews"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/29/8-best-practices-for-cisos-conducting-risk-reviews/"
date: "Wed, 29 Apr 2026 16:00:00 +0000"
author: "Rico Mariani"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<p class="wp-block-paragraph"><em><em>The Deputy CISO blog series is where&nbsp;<a href="https://www.microsoft.com/en-us/security/blog/topic/office-of-the-ciso/" rel="noreferrer noopener" target="_blank">Microsoft&nbsp;<em>Deputy Chief Information Security Officers</em></a>&nbsp;(CISOs) share their thoughts on what is most important in their respective domains. In this series, you will get practical advice, tactics to start (and stop) deploying, forward-looking commentary on where the industry is going, and more.</em></em> <em>In this blog, Rico Mariani, Deputy CISO for Microsoft Security Products, Research Infrastructure, and Engineering Systems shares some of his best practices and expertise in conducting risk reviews.</em></p>



<p class="wp-block-paragraph"><a href="https://www.microsoft.com/trust-center/security/secure-future-initiative/patterns-and-practices" rel="noreferrer noopener" target="_blank">The&nbsp;nature of cyberthreats has never been static</a>, but&nbsp;it’s&nbsp;hard to accurately convey the scale of&nbsp;their recent evolution and proliferation. As&nbsp;we’ve&nbsp;seen in many other arenas,&nbsp;AI has become&nbsp;a&nbsp;very&nbsp;powerful&nbsp;productivity tool for would-be cybercriminals. Between April&nbsp;2024 and April&nbsp;2025, Microsoft stopped&nbsp;$4 billion&nbsp;in fraud attempts.<sup>1</sup>&nbsp;And as of the&nbsp;writing of&nbsp;the&nbsp;<a href="https://www.microsoft.com/corporate-responsibility/cybersecurity/microsoft-digital-defense-report-2025/" rel="noreferrer noopener" target="_blank">Microsoft Digital Defense Report 2025</a>, we&nbsp;are tracking&nbsp;100&nbsp;trillion security signals each day&nbsp;(a 40% increase since&nbsp;2023).<sup>2</sup></p>



<div class="wp-block-buttons is-content-justification-center is-layout-flex wp-container-core-buttons-is-layout-a89b3969 wp-block-buttons-is-layout-flex">
<div class="wp-block-button has-custom-width wp-block-button__width-75"><a class="wp-block-button__link wp-element-button" href="https://www.microsoft.com/en-us/trust-center/security/secure-future-initiative" rel="noreferrer noopener" target="_blank">Explore the Microsoft Secure Future Initiative</a></div>
</div>



<p class="wp-block-paragraph">This is why I&nbsp;decided to&nbsp;write a blog&nbsp;about risk reviews. By asking the right questions, risk reviews help us transform the utility of our security data from primarily reactive remediation and response information into key insights helping to inform our proactive security stances. And embracing strong proactive security is something we can all do to mitigate our increased exposure to security threats.&nbsp;&nbsp;</p>



<p class="wp-block-paragraph">Risk reviews are also a topic I’ve lent focus to during my first six months as Deputy CISO for Microsoft Security. It’s a very interesting role for me, as I’ve traditionally described myself as performance specialist and a systems specialist more than a security specialist. It’s not necessarily a distinction of skill set, but more one of mindset, and what I’d like to share with you is actually a bit of a synthesis of my inherent performance- and systems-first way of thinking and things I’ve brought into that practice after working with many of the other Microsoft Deputy CISOs over the last few months.</p>



<p class="wp-block-paragraph">There are roughly eight points I want to bring up concerning risk reviews in this blog. Each point has the potential to help expose potential security vulnerabilities when brought up with security teams. Together, they represent a structured and approachable way to initiate necessary conversations and drive meaningful results:</p>



<ol class="wp-block-list" start="1">
<li class="wp-block-list-item">Assets</li>



<li class="wp-block-list-item">Applications&nbsp;</li>



<li class="wp-block-list-item">Authentication&nbsp;</li>



<li class="wp-block-list-item">Authorization&nbsp;</li>



<li class="wp-block-list-item">Network isolation&nbsp;</li>



<li class="wp-block-list-item">Detections&nbsp;</li>



<li class="wp-block-list-item">Auditing&nbsp;</li>



<li class="wp-block-list-item">Things not to&nbsp;miss&nbsp;</li>
</ol>



<p class="wp-block-paragraph">Now, why did I choose to highlight these areas and not others? Generally, I find that looking at problems from the lens of risk management gives me a fresh perspective. When you very consistently ask specific questions around these areas, they often effectively start the conversation you want to have.</p>



<p class="wp-block-paragraph">Just one last thing before we dive in: What I’m about to tell you is only approximately correct. There will be edge cases and exceptions, but generally I think you’ll find this information helpful.</p>



<h2 class="wp-block-heading" id="1-assets">1. Assets</h2>



<p class="wp-block-paragraph">The best place to start a review is identifying the assets that you need to protect. This will largely define the scope of the review. A good place to find those assets is, of course, on your architecture diagrams and your threat models. The assets we’re talking about could be storage (where perhaps you’re storing sensitive or otherwise important data) or they could be highly-privileged applications like command-and-control systems or something similar. This is, in short, the list of things that your cyberattacker wants to get to. </p>



<h2 class="wp-block-heading" id="2-applications">2. Applications</h2>



<p class="wp-block-paragraph">In the next step, you identify your applications. These are, broadly speaking, the active part of your system. They are the outward-facing surfaces that customers will use and the set of microservices that support your interface. These systems could be providing any set of services that you might need—and herein lies the problem. It’s entirely normal for your applications to require access to your most important assets, but that means the applications themselves can become viable targets for a cyberattacker. So how do we make this situation better? At this point, it’s reasonable to start talking about possible controls. </p>



<p class="wp-block-paragraph">Read up on&nbsp;<a href="https://review.learn.microsoft.com/security/zero-trust/sfi/zero-trust-source-code-access" rel="noreferrer noopener" target="_blank">Zero Trust for&nbsp;source&nbsp;code&nbsp;access</a>.</p>



<h2 class="wp-block-heading" id="3-good-quality-authentication">3. Good&nbsp;quality&nbsp;authentication </h2>



<p class="wp-block-paragraph">The next thing you will want to inspect is the form of authentication that your system is using. The best systems are using tokens for authentication, and they are getting these tokens from standard token issuers like, for instance, <a href="https://www.microsoft.com/en-us/security/business/microsoft-entra" rel="noreferrer noopener" target="_blank">Microsoft Entra</a>. It’s sometimes viable to have your own token generation system, but remember that such systems tend to have bugs. Those bugs can be exploitable. And even lacking bugs, there could be, say, gaps or vulnerabilities in your token issuing system such that perhaps the tokens cannot be properly scoped. The tokens could also tend to be too long-lived, or difficult to be made fine-grained enough, or lack the capacity to allow for flowing user context from the request to the authorization system. Many such deficiencies are possible. </p>



<div class="wp-block-buttons is-content-justification-center is-layout-flex wp-container-core-buttons-is-layout-a89b3969 wp-block-buttons-is-layout-flex">
<div class="wp-block-button has-custom-width wp-block-button__width-50"><a class="wp-block-button__link wp-element-button" href="https://www.microsoft.com/en-us/security/business/microsoft-entra" rel="noreferrer noopener" target="_blank">Explore Microsoft Entra solutions</a></div>
</div>



<p class="wp-block-paragraph">Even with a good quality token issuing system, you can easily find yourself in a situation where the tokens that you’re creating are too fungible, or too powerful, or both. Thinking back to the assets you’re trying to protect and the applications that you have, you can likely categorize some of the applications as having more “power,” if you will, than others. Sometimes we call these &#8220;highly privileged applications” because they have the capability to do something that is especially of interest to cyberattackers, like reading a lot of data, changing configuration, or anything like that. </p>



<p class="wp-block-paragraph">To best manage the privileges associated with these applications, it needs to be the case that the kinds of tokens that they use are as limited as possible. So, a particular token might authorize a capability for a certain customer, on behalf of a certain user, for a certain set of data—and nothing more than that. When privileges are very generic, like “I can do this operation for anyone, anywhere,” things become much more dangerous. So, here the idea is to make sure that the tokens that you’re getting are very specific to the intent that you have and that only the applications that need those tokens can get them, and, again, the tokens are as limited as possible. This goes a long way in reducing the possible damage that a cyberattacker could do if they found such a token errantly stored somewhere. </p>



<p class="wp-block-paragraph">A lot of the things we think about when we’re working with tokens and trying to limit them fall into the category of limiting what a&nbsp;cyberattacker&nbsp;can do if they get a foothold somewhere. This is the&nbsp;<a href="https://www.microsoft.com/en-us/security/business/security-101/what-is-zero-trust-architecture" rel="noreferrer noopener" target="_blank">Zero Trust</a>&nbsp;model,&nbsp;where you assume breach everywhere. &nbsp;</p>



<div class="wp-block-buttons is-content-justification-center is-layout-flex wp-container-core-buttons-is-layout-a89b3969 wp-block-buttons-is-layout-flex">
<div class="wp-block-button has-custom-width wp-block-button__width-75"><a class="wp-block-button__link wp-element-button" href="https://www.microsoft.com/en-us/security/business/zero-trust" rel="noreferrer noopener" target="_blank">Strengthen security with a Zero Trust strategy</a></div>
</div>



<p class="wp-block-paragraph">Additionally, it’s essential to use standard libraries to accurately authenticate with tokens, so that all the aspects and limitations of the token are certain to be honored. </p>



<p class="wp-block-paragraph">Learn&nbsp;about&nbsp;<a href="https://learn.microsoft.com/security/zero-trust/sfi/phishing-resistant-mfa" rel="noreferrer noopener" target="_blank">phishing-resistant multifactor authentication</a>&nbsp;from the Microsoft Secure Future Initiative (SFI).&nbsp;</p>



<h2 class="wp-block-heading" id="4-good-quality-authorization">4. Good&nbsp;quality&nbsp;authorization &nbsp;</h2>



<p class="wp-block-paragraph">Good quality tokens are not going to help you if they’re enforced poorly (or not at all). And bugs can creep into code. Ad hoc authorization code can render the good authentication that you’ve done moot. </p>



<p class="wp-block-paragraph">Any time you can use declarative style patterns that help you verify tokens against incoming APIs and the data that the client is attempting to access with your API, you’ll find yourself in a better place. Simple, consistent authorization yields fewer bugs and therefore less risk. </p>



<h2 class="wp-block-heading" id="5-network-isolation">5. Network&nbsp;isolation </h2>



<p class="wp-block-paragraph">In addition to having good quality tokens, it’s important to isolate the pieces of your environment to the maximum extent possible. Again, this is done because it’s prudent to assume that a cyberattacker has a foothold somewhere in your network. The questions are “where exactly can that foothold be,” and “once they have that foothold, where in my network can they get to?” If a threat actor can reach any part of your system from any other part of your system, this is obviously less good than if your most sensitive systems can be accessed from exactly one or two key places and nowhere else. When properly controlled, most footholds become useless to a cyberattacker—or at least only indirectly useful.  </p>



<p class="wp-block-paragraph">Use&nbsp;<a href="https://learn.microsoft.com/en-us/azure/virtual-network/service-tags-overview" rel="noreferrer noopener" target="_blank">service tags</a>&nbsp;to create boundaries around your various assets such that applications are used by exactly those systems that are supposed to be using&nbsp;them&nbsp;and data is accessed by exactly those applications that are supposed to be accessing the data. This goes a long way to take many cyberthreats off the table. &nbsp;</p>



<p class="wp-block-paragraph">Network isolation can happen at several levels in the network stack. Popularly, level 7 is used at the perimeter. Maybe this manifests as some kind of HTTP proxy, for example, or an HTTP routing gateway. However, protection is incomplete without additional work happening at level 3 within your network. You want to limit IP traffic to be going to exactly the places that you want it to go. You might use techniques like virtual LANs, or similar constructs like network security groups (NSGs) in <a href="https://azure.microsoft.com/en-us">Microsoft Azure</a>. The idea is to limit connectivity to exactly what is necessary to do the job and not give the cyberattacker freedom to move around. </p>



<p class="wp-block-paragraph">With good network isolation comes the ability to log any&nbsp;attempts to gain access at the perimeter,&nbsp;and potentially even internally. Depending on what networking technology&nbsp;you’re&nbsp;using, all of this is great for hunting.&nbsp;We’ll&nbsp;talk about that in the next section. &nbsp;</p>



<p class="wp-block-paragraph">Learn more about&nbsp;<a href="https://www.microsoft.com/security/blog/2025/10/07/new-microsoft-secure-future-initiative-sfi-patterns-and-practices-practical-guides-to-strengthen-security/" rel="noreferrer noopener" target="_blank">network isolation and other best practices from SFI</a>.</p>



<h2 class="wp-block-heading" id="6-detections">6. Detections &nbsp;</h2>



<p class="wp-block-paragraph">It’s normal to think about&nbsp;monitoring for&nbsp;reliability. Systems need to stay within their operating parameters in the face of changes and external conditions. But it’s also important to think about detection from the perspective of your threat model. If you identify five or ten risks in your threat model that need controls, it’s useful to think about how you might detect if any of those things are actually happening in your environment. &nbsp;</p>



<p class="wp-block-paragraph">In this context, one place to look is at the perimeter—by&nbsp;examining your incoming HTTP traffic, for instance. But you can also look anywhere in your environment where you&nbsp;predict&nbsp;that attacks might happen. You might look for badly formatted requests, or fuzzing, or evidence of DDoS attack—whatever is appropriate to the risks you have. The idea is that you want to be able to create alerts if you have evidence of a&nbsp;threat&nbsp;actor operating in your estate. &nbsp;</p>



<p class="wp-block-paragraph">And, of course, security products can be very helpful here. &nbsp;</p>



<h2 class="wp-block-heading" id="7-auditing">7. Auditing</h2>



<p class="wp-block-paragraph">We separate the notions of auditing from detection. Specifically, auditing is what I will call the pieces of data that you would use after a breach to determine the extent of the breach and the customers that were affected by it. In the event that you find a vulnerability without any evidence of threat actor exploitation, you’d want to go and check your auditing again to verify those claims. That way you can have evidence that whatever problem you found was not in fact exploited. If it was exploited, you’ll know to what extent, who was affected, and who needs to be notified. </p>



<p class="wp-block-paragraph">Some parts of your <a href="https://www.microsoft.com/en-us/security/business/security-101/what-is-edr-endpoint-detection-response" rel="noreferrer noopener" target="_blank">endpoint detection and response (EDR)</a> stream will be very useful for auditing. Additional auditing information can come from the logs you create in your applications that record suitable information concerning recent activity. </p>



<h2 class="wp-block-heading" id="8-things-not-to-miss">8. Things not to&nbsp;miss </h2>



<p class="wp-block-paragraph">It’s important to think about all the applications and data that you have in your estate. For instance, it’s easy to overlook the backup data that you have stored. A cyberattacker might not be able to get access to your primary systems but might find that your backups are entirely unprotected and they can just read the backup.</p>



<p class="wp-block-paragraph">Similarly, support systems often go overlooked. There are frequently important <a href="https://www.microsoft.com/en-us/security/blog/2025/10/15/the-importance-of-hardening-customer-support-tools-against-attack/" rel="noreferrer noopener" target="_blank">customer support scenarios</a> that require access, and it’s easy to fall into the trap of not giving those systems the highest level of scrutiny. </p>



<p class="wp-block-paragraph">We should add systems that are under development and test systems to this problematic set. In both these cases, the code that’s running those systems is less trustworthy than normal production code. Development code, for instance, can be presumed to have more bugs than production code. Some of those bugs might be authorization bugs. And if there are authorization bugs, that buggy code might provide access to important assets. Therefore, your plans should include even greater scrutiny when it comes to these kinds of systems. </p>



<p class="wp-block-paragraph">Explore&nbsp;<a href="https://www.microsoft.com/trust-center/security/secure-future-initiative/patterns-and-practices" rel="noreferrer noopener" target="_blank">actionable patterns and practices from SFI</a>.&nbsp;</p>



<h2 class="wp-block-heading" id="in-summary">In summary</h2>



<p class="wp-block-paragraph">If you&#8217;ve gotten as far as identifying all of your assets, all your applications, and then thinking about the access patterns and controls that you have between them—including authentication, authorization, network isolation, and the use of bug-resistant patterns—you&#8217;re in a pretty good place to write a risk summary that can guide your actions for many months. And we haven&#8217;t even touched on basic things like vulnerability management, security, bug management, and the usual software lifecycle things that are necessary to keep the system in good health. Combine all of the above and you should have a good-looking risk plan. </p>



<div class="is-style-inline-centered wp-block-bloginabox-theme-promotional">
	
<div class="promotional promotional--has-media promotional--media-right">
	<div class="promotional__wrapper">
		<div class="promotional__content-wrapper">
			<div class="promotional__content">
				

<h2 class="wp-block-heading has-h-2-font-size" id="microsoftdeputy-cisos">Microsoft<br />Deputy CISOs</h2>



<p class="has-h-5-font-size wp-block-paragraph"><strong>To hear more from Microsoft Deputy CISOs, check out the&nbsp;<a href="https://www.microsoft.com/en-us/security/blog/topic/office-of-the-ciso/" rel="noreferrer noopener" target="_blank">OCISO blog series</a></strong>:</p>



<p class="wp-block-paragraph"></p>



<p class="wp-block-paragraph">To stay on top of important security industry updates, explore resources specifically designed for CISOs, and learn best practices for improving your organization’s security posture, join the&nbsp;<a href="https://info.microsoft.com/ww-landing-subscribe-to-ciso-digest.html?lcid=en-us" rel="noreferrer noopener" target="_blank">Microsoft CISO Digest distribution list</a>.</p>

			</div>
		</div>
					<div class="promotional__media-wrapper">
				<div class="promotional__media">
											<img alt="Man with smile on face working with laptop" class="attachment-full size-full" height="370" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/10/Man-with-smile-on-face-working-with-laptop-2.webp" width="748" />									</div>
			</div>
			</div>
</div>
</div>



<p class="wp-block-paragraph">To learn more about Microsoft Security solutions, visit our <a href="https://www.microsoft.com/en-us/security/business" rel="noreferrer noopener" target="_blank">website.</a> Bookmark the <a href="https://www.microsoft.com/security/blog/" rel="noreferrer noopener" target="_blank">Security blog</a> to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (<a href="https://www.linkedin.com/showcase/microsoft-security/" rel="noreferrer noopener" target="_blank">Microsoft Security</a>) and&nbsp;X&nbsp;(<a href="https://twitter.com/@MSFTSecurity" rel="noreferrer noopener" target="_blank">@MSFTSecurity</a>) for the latest news and updates on cybersecurity.&nbsp;</p>



<hr class="wp-block-separator has-alpha-channel-opacity" />



<p class="wp-block-paragraph"><sup>1</sup><a href="https://www.microsoft.com/security/blog/2025/04/16/cyber-signals-issue-9-ai-powered-deception-emerging-fraud-threats-and-countermeasures/" rel="noreferrer noopener" target="_blank">Microsoft Cyber Signals Issue 9</a>.&nbsp;</p>



<p class="wp-block-paragraph"><sup>2</sup><a href="https://www.microsoft.com/security/security-insider/threat-landscape/microsoft-digital-defense-report-2024" rel="noreferrer noopener" target="_blank">Microsoft Digital Defense Report 2024</a>.</p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/29/8-best-practices-for-cisos-conducting-risk-reviews/">8 best practices for CISOs conducting risk reviews</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
