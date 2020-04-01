# Engineers Way of Working
## Purpose of this document

_Here, the engineers of Nuuday publish their agreed ways of working. The goal is to achieve complete squad autonomy while protecting Nuuday assets. Assets under protection are listed in full under "Asset Classes". While this document outlines what must be done for Nuuday assets, the document is written to provide solid and practical guidelines for squads also._

## Our Oath
In order to defend and preserve the honor of the profession of computer programmers, I Promise that, to the best of my ability and judgement:

1. I will not produce harmful code.
2. The code that I produce will always be my best work. I will not knowingly allow code that is defective either in behavior or structure to accumulate.
3. I will produce, with each release, a quick, sure, and repeatable proof that every element of the code works as it should.
4. I will make frequent, small, releases so that I do not impede the progress of others.
5. I will fearlessly and relentlessly improve my creations at every opportunity. I will never degrade them.
6. I will do all that I can to keep the productivity of myself, and others, as high as possible. I will do nothing that decreases that productivity.
7. I will continuously ensure that others can cover for me, and that I can cover for them.
8. I will produce estimates that are honest both in magnitude and precision. I will not make promises without certainty.
9. I will never stop learning and improving my craft.

Ref. [https://blog.cleancoder.com/uncle-bob/2015/11/18/TheProgrammersOath.html](https://blog.cleancoder.com/uncle-bob/2015/11/18/TheProgrammersOath.html)

## Assets
### Protected Assets
_The primary protected assets are defined as **data**, **public APIs**, and **public documentation**. Asset ownership and asset versioning applies to all the protected assets._
 
### Data 
- Definition: _Data_ is any piece of data accessible through a _Public API_. The integrity of Nuuday data is considered part of Nuuday Intellectual Property (IP) and will be protected by members of the organisation. 
- Description: 
	- Data is assumed to be Nuuday's competitive advantage. This is Nuuday's mote that protects us from competitors. ([Reference](https://www.re-thinkwealth.com/moat-warren-buffetts-terms-important/) or [cached reference](https://webcache.googleusercontent.com/search?q=cache:EhJMR97TpY0J:https://www.re-thinkwealth.com/moat-warren-buffetts-terms-important/)) 
	- The only access to our Data will be through _Public API_.
- Roadmap: Data roadmap should evolve over time like all other assets. Gaps identified and filled, inconsistencies fixed.

### Public APIs 
- _Public APIs_ are those published on [https.//api.nuuday.dk](https://api.nuuday.dk). All other API are considered private and subject to change without notice. 
- Description: 
	- Squads can only communicate through API published on [https.//api.nuuday.dk](https://api.nuuday.dk).
	- The API defined is understood as public. Those API are considered an asset of Nuuday. The API will respect symentic versioning as outline on [https://semver.org/](https://semver.org/)
	- Changes to the X component of the version number will require detailed review by senior engineer
	- The implementation of the API is the responsbility of the owning Tribe. The implementation can and should change as necessary without formal review external to that owning tribe.

### Public documentation
- Definition: 
	- _Public documentation_ is any documentation accompanying a _Public API_ or any large area documentation available under [https.//docs.nuuday.dk](https://docs.nuuday.dk)
	- _Public documentation_ describes the public interface for an application. Internal documentation is not a subject for this document, but documentation is always kept with the asset source code. 
- Documentation is only mandated on Public API and our Data.

## Asset Versioning
- All assets are to be versions using [https://semver.org/](https://semver.org/)

## Asset Ownership
- Ownership of an asset is documented in the CODEOWNERS file of the application git repo. 
- Every asset will have an owner or owner group, that is also understood to be maintainer. 

## Technologies
We use the [ThoughtWorks](https://www.thoughtworks.com/radar) concept of a _technology radar_ to inform teams which technologies are most widely used in Nuuday. 

We use the following description for the rings of the radar. The radar is open for all in Nuuday, however only those with operational responsibility for production systems will promote technologies through the radar.

- Total Cost of Ownership is the responsibility of this same group

- Classification: 
	- **ADOPT** — Technologies we have high confidence in to serve our purpose, also in large scale. Technologies with a usage culture in our Nuuday production environment, low risk and recommended to be widely used.
	- **TRIAL** — Technologies that we have seen work with success in project work to solve a real problem; first serious usage experience that confirm benefits and can uncover limitations. TRIAL technologies are slightly more risky; some engineers in our organisation walked this path and will share knowledge and experiences.
	- **ASSESS** — Technologies that are promising and have clear potential value-add for us; technologies worth to invest some research and prototyping efforts in to see if it has impact. ASSESS technologies have higher risks; they are often brand new and highly unproven in our organisation. You will find some engineers that have knowledge in the technology and promote it, you may even find teams that have started a prototyping effort.
	- **HOLD** — Technologies not recommended to be used for new projects. Technologies that we think are not (yet) worth to (further) invest in. HOLD technologies should not be used for new projects, but usually can be continued for existing projects.

## Operations

**DevSecOps** is the primary method we run operations. 

Services should be:
- **Auditable**: Provide an audit trail for all changes to the production environment. For Example:
	- Follow GitOps – descriptive and traceable definitions of applications and environments
	- Document every manual step, no exceptions 
- **Observable**: Logging must be available in a central logging application. It has to be all accessible by every member of the owning squad. For Example:
	- Logstash
	- Kibana 
	- Support open metrics - and have the capability to apply measurements anywhere in the application. 
- **Low Risk**: Changes must be tested on a subset of customers in production before exposing those changes to all customers. For Example: 
	- Canary deployments
	- Feature toggling 
	- Short mean time to recovery (MTTR)
- **Measurable**: SLOs and SLIs must be defined for the application 
	- [https://cloudplatform.googleblog.com/2018/07/sre-fundamentals-slis-slas-and-slos.html](https://cloudplatform.googleblog.com/2018/07/sre-fundamentals-slis-slas-and-slos.html) 
	- Based on the definition and tracking of SLOs each asset owning squad will have a defined set of regulations enforcing a return to healthy SLIs as fast as possible. 
- **Alerting**: Must be configured and notify directly to the owning squad when the application falls outside agreed Service Level Objective window. This does **not** mean that traditional Operations / MiM / Control Center / NOC should be cut out of the loop. 
	- Prometheus 
	- AppDynamics

## Costs
- No technology should be introduced without producing a MVP reference implementation first. 
- Should the cost of the reference implementation exceeded 100.000 DKK, the technology is dropped and cost accepted as sunk cost. 
- This does not make it impossible to spend above that amount on an asset - but we _have_ to be able to demonstrate value in small increments. 

## Asset Lifecycle
- No asset can be over 4 years old from first commit. 
- All assets over 4 years are scheduled for decomm and automatically tagged as deprecated on [https.//api.nuuday.dk](https://api.nuuday.dk)

## Technical Debt
- All Assets require active development to reduce the risk of technical debt building up. SLIs/SLOs are used as indicators of "debt" 
- When it becomes too expensive to operate within the SLO window, the service is considered to be technically too expensive and will be rewritten. Reference: [https://cloudplatform.googleblog.com/2018/07/sre-fundamentals-slis-slas-and-slos.html](https://cloudplatform.googleblog.com/2018/07/sre-fundamentals-slis-slas-and-slos.html)
- Technical debt that refers to _Data_ is the responsibility of the architect chapters across Nuuday. 

## Security
- Each service must be registered with a CMDB service. This can be through a push or pull mechanism.
- All services must use best efforts run with the latest version of dependencies possible to limit security concerns from 3rd party libraries. 
	- Build pipelines must do container scanning - or utilise enforced scanning in the repository. Similar scanning is required for non-container pipelines. 
	- Applications should use tools like Snyk or JFrog Xray for scanning as they alert teams after release about vulnerabilities. 
- No manual changes allowed.
- No elevated privileges allowed on VMs.
- Use docker in ROOTLESS mode when possible.
- Services must use JWT tokens issues by an IDP. Username/password are not permitted. 
- Run HTTPS everywhere. 
- Never put plain text secrets into VCS. Ideally use a KeyVault that manage secret lifecycle. 
- Use the Nuuday FortiGate firewall. 
- Use network microsegmentation.

## Privacy
- Never log PPI data in log files. Use masks to hide data where necessary. This is a feature available on most modern logging frameworks.
- Each squad needs at least one member aware of GDPR
- Each squad needs an awareness of the difference between anonymisation of data, pseudonymisation of data and raw data as it is described in GDPR legislation. 
- Encrypt all data at rest. 
