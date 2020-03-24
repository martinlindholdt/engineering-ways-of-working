# engineering-ways-of-working

# Purpose of this document
Here, the engineers of Nuuday publish their agreed ways of working. The goal is to achieve complete squad automonoy while protecting Nuuday assets. Assets under protection are listed in full under "Asset Classes". While this document outlines what must be done for Nuuday assets, the document is written to provide solid guidelines for squads also.

# Assets
- Public APIs
  - Public API are those published on [https.//api.nuuday.dk](https://api.nuuday.dk). All other API are consider private and subject to change without notice
- Public documentation
  - Public documentation is any documentation accompanying a Public API or any large area documentation available under [https.//docs.nuuday.dk](https://docs.nuuday.dk)
  - Public documentation describes the public interface for an application. Internal documentation is not a subject for this document

# Asset Versioning
- All assets are to be versions using [https://semver.org/](https://semver.org/)

# Asset Ownership
- Ownership is documented in the OWNERS file of the application git repo
- Every asset will have an owner that is also understood to be maintainer

## Public API
- Squads can only communicate through API published on [https.//api.nuuday.dk](https://api.nuuday.dk).
- The API defined is understood as public. Those API are considered an asset of Nuuday. The API will respect symentic versioning as outline on [https://semver.org/](https://semver.org/)
- Changes to the major number will require detailed review by senior engineers
- The implementation of the API is the responsbility of the owning Tribe. This can and should change as often as required

## Public Documentation
- Documenation is only mandated on public interfaces

# Technologies
- We use the thoughtworks concept of tech-radar to information teams which technologies are most used in Nuuday. We use the following description for the rings of the radar. The radar is open for all in Nuuday however only those with operational responibility for production systems will promote technologies through the radar.
- Total Cost of Ownership is the responsiblity of this same group

- ADOPT — Technologies we have high confidence in to serve our purpose, also in large scale. Technologies with a usage culture in our Zalando production environment, low risk and recommended to be widely used.
- TRIAL — Technologies that we have seen work with success in project work to solve a real problem; first serious usage experience that confirm benefits and can uncover limitations. TRIAL technologies are slightly more risky; some engineers in our organization walked this path and will share knowledge and experiences.
- ASSESS — Technologies that are promising and have clear potential value-add for us; technologies worth to invest some research and prototyping efforts in to see if it has impact. ASSESS technologies have higher risks; they are often brand new and highly unproven in our organisation. You will find some engineers that have knowledge in the technology and promote it, you may even find teams that have started a prototyping effort.
- HOLD — Technologies not recommended to be used for new projects. Technologies that we think are not (yet) worth to (further) invest in. HOLD technologies should not be used for new projects, but usually can be continued for existing projects.

# Operations
- DevSecOps is the primary method we run operations
Services should be:-
- **Auditable**: Provide an audit trail for all changes to the production environment. For Example:
  - Follow GitOps – descriptive and traceable definitions of applications and environments
  - Document every manual step, no exceptions
- **Observable**: Logging must be available in a central logging application all accessible by all members of the owning squad. For Example
  - Logstash
  - Kibana
  - Support open metrics
- **Low Risk**: Changes must be tested on a subset of customers in production before exposing those changes to all customers. For example
  - Canary deployments
  - Feature toggling
  - Short mean time to recovery (MTTR)
- **Measurable**: SLO and SLI must be defined for the application
  - [https://cloudplatform.googleblog.com/2018/07/sre-fundamentals-slis-slas-and-slos.html](https://cloudplatform.googleblog.com/2018/07/sre-fundamentals-slis-slas-and-slos.html)
- **Alerting**: must be configured and notify directly to the owning squad when the application falls outside agreed Service Level Objective window. This does not mean that traditional Operations / MiM / Control Center / NOC should be cut out of the loop
  - Prometheus
  - AppDynamics

# Costs
- No technology should be introduced without producing a MVP reference implementation first
- Should the cost for the reference exceeded 100.000 DKK, the technology is drop and cost accepted as sunk cost

# Asset Lifecycle
- No asset can be over 4 years old from first commit
- All assets over 4 years are scheduled for decomm and automatically tagged as depricated on [https.//api.nuuday.dk](https://api.nuuday.dk)

# Technical Debt
- All Assets require active development to reduce the chances of technical debt building up. SLI/SLO are used as indicators of "debt"
- Technical debt as it refers to data is the responsiblity of the architect chapters across Nuuday

# Security
- Build pipelines must do container scanning
- Use the Forigate firewall
- Use network microsegmentation
- Use JWT tokens issues by IDP

# Privacy
- Encrypt data at rest
