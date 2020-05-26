## Analysis of Australian Government’s COVIDsafe application employing SOAP concepts

*A group of researchers from disciplines across the ANU have collaborated on a draft framework for evaluating various contact tracing technologies. The aim of the framework is to make explicit factors that are relevant to the design and deployment of any technology implementation, and to surface relationships between these factors. The white paper outlining the framework, styled around the acronym SOAP,  is available [here](https://github.com/anu-act-health-covid19-support/SOAP-evaluation/blob/master/SOAP%20method%20for%20evaluating%20contact%20tracing_for%20publication.pdf). SOAP takes into account: the extent to which an implementation is **secure** from malicious or inadvertent misuse; how suitable it is for **operational use** as part of a health authority’s response to an outbreak; the extent to which **assurance** of that technology - validating that it is working as intended - is possible; and the extent to which it preserves individual and community privacy.*

This document applies draft SOAP concepts to the Australian Government’s [COVIDSafe](https://www.health.gov.au/resources/apps-and-tools/covidsafe-app) contact tracing mobile application. It provides readers with a general overview of considerations associated with the deployment of COVIDSafe as a supporting tool for existing contact tracing processes. Advantages and drawbacks of the application are made clear, along with tradeoffs in its design and implementation. The process of working through these concepts provides a path to validating and formalising the draft SOAP framework. This analysis is based on accessible information on the web, including that produced by other researchers, and is current as of 6 May 2020.

*As at the time of writing, the source code for the COVIDSafe application had not been made publicly available. New information about how COVIDSafe functions emerges every day. The Australian government has stated that the app is based on the Singapore Government’s TraceTogether Mobile Application, which in turn is based on the open BlueTrace protocol.*  
## The positives: a high level summary
* **The app takes demonstrable steps to preserve the privacy of individuals**: In adopting the Singapore Government’s TraceTogether application, the Australian government has made significant efforts to adhere to privacy-preserving principles. These practices set a strong privacy and security standard for any future government services engaging with individuals involving the collection of identifiable data. These include:
* Bluetooth data being stored locally on a user’s device, unless and until they choose to share it with public health authorities in their state or territory
* Data being stored locally in an encrypted form
* Data collected being minimal and only for the purpose of capturing devices in proximity to users of COVIDSafe
* Provision of a mechanism for users of the app to [request deletion of data](https://covidsafe-form.service.gov.au/) stored in the national data store
* Plain english consent screen for individuals, and plain english comprehensive FAQs and usage guidelines for citizens looking to learn more about COVIDsafe
* Users are offered the ability to delete data uploaded by them to the data store
* The government has published its exposure draft of a [Bill amending the Privacy Act 1988](https://www.ag.gov.au/RightsAndProtections/Privacy/Documents/exposure-draft-privacy-amendment-public-health-contact-information.pdf) to add safeguards surrounding the access to, use and deletion of COVIDSafe data, and introduces serious offences for breaches

* **There’s continuing investment in simple resources to explain how COVIDSafe works for the general public**: A range of [educational resources have been developed](https://www.health.gov.au/resources/apps-and-tools/covidsafe-app#about-the-app), and are being developed, to support individuals using COVIDSafe effectively and understanding what data is collected on their device, and how this will be used. Plain english efforts to explain data practices - what is collected, how it is collected, who has access to it - should be part of all data collecting services by government. 


* It has been designed to supplement, not replace, contact tracing: To mitigate issues of reliability of COVIDSafe data, COVIDSafe will simply supplement, not replace, contact tracing processes. This also ensures contact tracing processes still cover the widest possible range of individuals who may have been in contact with an infected case, irrespective of whether they are users of COVIDSafe.
## Areas to improve

There are always areas to improve. For each of the positive steps outlined above, legal scholars, security and privacy experts and user researchers have offered detailed advice on how these could continue to be refined. We note: Graham Greenleaf and Katherine Kemp’s [commentary on legal issues](https://t.co/PfgBqjayVE?amp=1); [Geoffrey )Huntley](https://twitter.com/GeoffreyHuntley), [Vanessa Teague](https://github.com/vteague/contactTracing) and [Jim Mussared](https://docs.google.com/document/d/1u5a5ersKBH6eG362atALrzuXo3zuZ70qrGomWVEC27U/edit)’s breakdowns of [technical privacy and security issues](https://docs.google.com/document/d/1u5a5ersKBH6eG362atALrzuXo3zuZ70qrGomWVEC27U/edit).

We do not try to offer an exhaustive list of all possible refinements to COVIDSafe. We focus on problems that:
* **Significantly impact COVIDSafe functionally working as intended**
* **Erode comprehension and trust in how COVIDSafe functions**
* **Pose serious security risks to individuals using the app**

These include:
* **No source code for COVIDSafe has been released as of 6 May 2020**. This has allowed confusion as to how COVIDSafe works to grow, and has forced researchers to reverse engineer and observe runtime behaviours of the app, speculating as to intentions behind differences between COVIDSafe and TraceTogether implementations.


* **Public messaging regarding the purpose and effect of downloading COVIDSafe may potentially mislead the public and increase infection rates**: COVIDSafe acts retrospectively. In the event a person tests positive for COVID-19, and is a user of COVIDSafe, they consent to share that data with contact tracers, who can then follow up with any other users of COVIDSafe who have been in proximity to that user for a prescribed period of time.** It does not shield individuals from contracting COVID19**. While metaphors like sunscreen and its shield logo offer a persuasive and immediate picture of how the app may help, they may mask practical limitations of the app and encourage over-confidence among the public.


* **It is not clear the extent to which COVIDSafe works for Apple users, and public messaging regarding issues with iOS has been confusing. Daily notifications to users of the COVIDSafe instructing them to keep the app open are not sufficiently clear**. The DTA has noted their intent to resolve iOS issues in the near future (although whether this changes the [basic functioning of COVIDSafe](https://www.theguardian.com/world/2020/may/06/covidsafe-app-is-not-working-properly-on-iphones-authorities-admit?CMP=Share_iOSApp_Other) for contact tracers is unclear)

* **The process following a user providing consent to upload data generated by COVIDSafe to the COVIDSafe National Data Store is opaque**. How access to that data is managed, the training contact tracers receive on interpreting that data, and details of storage and use, should be made available for public scrutiny, and to assure the integrity of the process. 

* **No metrics on usage of the app are available, besides summaries of app downloads**. It is not clear how success for the app is defined,  or in what circumstances it would be significantly revised or mothballed. Some metrics over time might include:
* Number of registrations with the app
* Number of uploads to the national data store
* Number of cases in which COVIDSafe data supports contact tracing 


* **Vulnerabilities in Android and iOS implementations may support long term tracking of devices**. See e.g. [here](https://docs.google.com/document/d/1u5a5ersKBH6eG362atALrzuXo3zuZ70qrGomWVEC27U/edit).


For a detailed consideration of issues and needs identified under each of SOAP (Security, Operational Use, Assurance and Privacy) please continue reading. Each heading responds to questions posed in the [SOAP whitepaper](https://github.com/anu-act-health-covid19-support/SOAP-evaluation).

## What is COVIDsafe?
COVIDSafe is a mobile application, developed by the Australian federal government, via their Digital Transformation Agency. It was released on Sunday, 26th April 2020, within Australia only, for both Android and iOS-based handsets. It is positioned as an augment to existing manual contact tracing efforts undertaken by health authorities across Australia. 

At a high level, a mobile phone with the COVIDSafe application installed uses Bluetooth to “handshake” with other handsets that are running COVIDSafe, and which are in close proximity for 15 minutes or more within 1.5 metres. The data around which other handsets a user has been in close proximity to is stored on the handset, and then released to health authorities only with the consent of the user. 

The processes and procedures used by health authorities once the data is obtained from the user are not openly available for analysis. This makes it hard to assess the extent to which warranties made to users in the context of downloading COVIDSafe are carried through in the event that user consents to the uploading of their data to the national data store.
## Security

*SOAP considers the confidentiality, integrity and availability of contact tracing systems. It examines both technical vulnerabilities, and vulnerabilities a system may have to fraud, misuse and abuse.*

COVIDSafe takes a range of steps to reduce security risks associated with Bluetooth data generated by the application, highlighted under positives on page 1. One key departure from the implementation of the Singapore Government’s TraceTogether app, however, which has been identified as a security vulnerability, is the lifetime of TempIDs. TempIDs uniquely identify specific devices for a prescribed period of time. TraceTogether rotates TempIDs every 15 minutes, and downloads TempIDs in batches. COVIDSafe obtains single TempIDs at a time from the server, at 2 hour intervals. The length of time for which these TempIDs remain the same may facilitate the identification of a device and long term location tracking. For a detailed discussion of issues associated with TempIDs and proposed fixes, see Mussared ([Issue 1](https://docs.google.com/document/d/1u5a5ersKBH6eG362atALrzuXo3zuZ70qrGomWVEC27U/edit#heading=h.q4sovraiy4kn)).

Like the Singapore Government solution, COVIDSafe is a *centralised* proximity-detecting solution. The design of COVIDSafe does not prevent contact tracers/health authorities knowing the identity of COVIDSafe users, as they are able to decrypt data logs and match these to the real identities of app users. The Singapore government, in designing their solution, [noted](https://blog.gds-gov.tech/automated-contact-tracing-is-not-a-coronavirus-panacea-57fb3ce61d98) that this was necessary to facilitate the process of contact tracing: namely, providing a phone number and name for human contact tracers to get in touch with.

Because COVIDSafe *is* centralised, the potential for human error or fraud in declaring COVID19 infection status is reduced. The use of COVIDSafe data is only triggered by requests from contact tracers, as part of existing contact tracing of already confirmed COVID19 cases. It should be noted, nonetheless, that the existence of an app may increase the likelihood of COVID-related phone scams, a number of which have already been identified on [ScamWatch](https://www.scamwatch.gov.au/types-of-scams/current-covid-19-coronavirus-scams) (over 2,000 scam reports with over $700,000 in reported losses).

No information regarding the National COVIDSafe data store, other than that AWS is supporting data storage, has been released. This makes it hard to assess security vulnerabilities associated with the data store. 

Validation mechanisms for ensuring that the data stored on a user’s phone, and the data uploaded from the phone to COVIDSafe, are equivalent, are unknown. This is not to say they don’t exist, but their efficacy cannot be assured at this time.

The Australian government has not disclosed information publicly about whether security and risk assessments have been conducted, and whether standard processes for documenting system requirements, and identifying and responding to cyber risks, implemented. They have not outlined testing that was undertaken prior to launch. The release of this information could assist security researchers assessing security vulnerabilities in the app. This does not mean security assessments haven’t been conducted, simply that they are not accessible on the open Web. 
## Operational Use

*SOAP considers the extent to which a contact tracing technology practically supports and improves existing contact tracing processes, whether it is straightforward for citizens to engage with and whether contact tracing experts have been involved in its design.*

5 million Australians have downloaded COVIDSafe in under two weeks, testament to the effectiveness of the government’s public awareness campaign. This is not a sufficient number of users, however, for COVIDSafe to be regularly or reliably employed by contact tracers as part of contact tracing processes. Even if 40 per cent of Australian citizens download the app, some experts have estimated that the likelihood of it identifying a contact in a random encounter is around [4 per cent](https://www.abc.net.au/news/science/2020-05-06/coronavirus-contact-tracing-app-covid-safe-lockdown-lift/12217146).

The Australian government drew on [the expertise of](https://www.innovationaus.com/atlassian-and-the-covidsafe-team/) several technology companies, consultancies and experts in designing COVIDSafe. It is not clear the extent to which state and territory health authorities, or contact tracers explicitly, were consulted in the development of COVIDSafe. [News reports](https://www.abc.net.au/news/2020-05-02/coronavirus-app-currently-not-fully-operational/12208924) indicate that as of 6 May 2020, the national data store is not yet operational and state and territory health authorities cannot access COVIDSafe data. The ease of use of the national data store to augment existing contact tracing processes is unclear.

COVIDSafe has sustained heavy criticism for issues preventing residents in Australia with devices registered to overseas [Google and Apple app stores from downloading the app](https://www.abc.net.au/news/2020-05-02/coronavirus-app-currently-not-fully-operational/12208924) (which requires users have an Australian account and phone number). Questions have also been raised about its effectiveness on iOS devices, both running in the background, and its compatibility with older iOS devices. Notifications to citizens who have downloaded the app, instructing them to ‘keep your Bluetooth on and the app open’, lack sufficient detail for users to understand what keeping the app open (in the foreground) means. 

Because COVIDSafe is designed to supplement existing contact tracing processes, it does not e.g. exclude certain populations from the contact tracing processes altogether. IDs supplement follow up phone interviews with that infected person. From that person, information about their location, recent movements and encounters are gathered to support contact tracing. As such, any people in proximity to an infected person who do not have COVIDSafe installed, or have a device full stop, may still be identified and alerted using existing processes.

Whether user testing and accessibility testing, and which cohorts or user profiles COVIDSafe was tested against, is unknown. It is unclear whether COVIDSafe aligns with the [Government Digital Service Standard](https://www.dta.gov.au/help-and-advice/digital-service-standard/digital-service-standard-criteria), which sets expectations about conducting user research. 

In the event COVIDSafe did achieve widespread adoption by Australian citizens, and became a more integral part of contact tracing processes, one potential downstream consequence may be the faster contact tracing of people in possession of smartphones. That is, with COVIDSafe facilitating speedier contact tracing, communities of disadvantage, who don’t possess smartphones, or rely on friends or family members to communicate for them, or did not install / could not install the app, may end up being deprioritised (in terms of time) with alerts to self-isolate.

The COVIDSafe application is currently only available in English. According to Australian Bureau of Statistics census data, over one in four Australians speaks a language other than English at home. 
## Assurance

*SOAP suggests that assurance mechanisms for organisations deploying an application to ensure it is working as intended, and can be interrogated and validated by independent parties, are an important part of evaluating the success of any contact tracing technology.*

As of 6 May 2020, source code and any accompanying technical documentation for COVIDSafe has not been released, although the Australian government has noted this will occur within weeks. It is not known what license the source code will be made available under, however the antecedent projects used by the Singaporean government in their TraceTogether app, are available under the copyleft GPLv3 license. 

It is not clear what processes exist for monitoring and evaluating the performance of COVIDSafe from publicly available sources. No definition of success, other than number of downloads, has been publicly stated. Clearer benchmarks of success, helping the government to ensure COVIDSafe is operating as intended, are essential, whether publicly expressed or internally visible. Metrics to measure use of COVIDSafe against could include:
* Number of registrations with the app
* Number of installations as a percentage of the number of registered handsets in Australia 
* Number of uploads to the national data store
* Number of cases in which COVIDSafe data supports contact tracing 
* The current “time to trace” metric using manual methods, and how much that is expedited using the data in the COVIDSafe National Data Store 

The Australian government has noted that data associated with COVIDSafe will be deleted, and the app shut down, when the pandemic is over. How and when this will be decided is unclear. Oversight of the stated commitment to disable COVIDSafe functionality, and delete user data on request, is not clear.

Auditing methods have not been defined publicly. COVIDSafe technical support web pages provide no mechanism to provide feedback. Road maps with details regarding future enhancements have not been made publicly available for comment. 
## Privacy

*SOAP requires that data collected via a contact tracing app be proportionate to its purpose, and exercise a range of privacy safeguards.* 

Downloading of COVIDSafe is voluntary. Consent is sought from users downloading the app to exchange Bluetooth signals with nearby phones running the same app.

Consent screens and [privacy FAQs](https://www.health.gov.au/using-our-websites/privacy/privacy-policy-for-covidsafe-app) on the COVIDSafe website are simple and clear. COVIDSafe stores only the following identity information: a user’s mobile number and a random UserID, along with age group and postcode collected upon registration. Bluetooth IDs exchanged are stored on a user’s device. If they test positive for COVID19, they consent to provide their state or territory health authority with the data stored on that device by the app. This information is added to information - location, demographic details, identities of contacts - collected by health officials from the user of the app during phone interviews. 

Users can delete the app at any time. The COVIDSafe website states that data will only be used for contact tracing in COVID-19. No processes for deleting data already provided during contact tracing are stated. It is not clear what mechanisms exist to assure users that data is only being used for contact tracing as stated, or what this expressly means. 

There are protections in the privacy policy for COVIDSafe against coercion, which is prohibited under the Biosecurity Determination. The government has published its exposure draft of a [Bill amending the Privacy Act 1988](https://www.ag.gov.au/RightsAndProtections/Privacy/Documents/exposure-draft-privacy-amendment-public-health-contact-information.pdf), to add safeguards surrounding the access to, use and deletion of COVIDSafe data, and which introduces serious offences for breaches.

The complaints mechanism or mechanisms for users wishing to raise concerns about misuse or abuse of COVIDSafe policies is not clear. Whether users seek redress from the OAIC, or the Fair Work Commission, or Australian Human Rights Commission, regarding coercion, is unclear. 

Data logs have been removed from view for users of the application, making it difficult for users to access their own data, as well as consent to upload it to the National Data Store. This could have been perceived as a privacy risk because, in storing records of “hand shakes” on device, their logs by default reflect other devices in proximity. 

Privacy risks posed by security vulnerabilities in COVIDSafe are documented under the heading ‘Security’ of this document, and further discussion of technical privacy risks [here](https://docs.google.com/document/d/1u5a5ersKBH6eG362atALrzuXo3zuZ70qrGomWVEC27U/edit#heading=h.q4sovraiy4kn).

The Australian government Department of Health commissioned law firm Maddocks to undertake a privacy impact assessment (PIA) of the COVIDSafe application. Maddocks found that the government had demonstrated a “genuine appreciation of the importance of addressing privacy considerations and risks involved in developing and implementing the App”, but that further work was required to address potential privacy risks, including the provision of clarity on exactly what information was being collected by the App, the provision of assurances that data collected through the App would only be used for contact tracing (and not, for example, the enforcement of public health social distancing orders), and the provision of further assurance around security risks.

It is not clear what current independent oversight mechanisms - whether OAIC, FWC, or AHRC - could functionally oversee that COVIDSafe is working effectively. That is, as well as safeguarding privacy, that it practically aids contact tracing. It is also not clear what oversight mechanism could be triggered to assure citizens that longer term concerns around misuse of COVIDsafe data - e.g. the linking of data contained in the National Data Store with other data sources - will not eventuate. 


