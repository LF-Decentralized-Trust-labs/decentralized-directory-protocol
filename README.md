# Decentralized Directory (DeDi): Enhancing Trust in Digital Transactions

## The Problem: Slow, Expensive Verification. High-cost of establishing trust!

> DeDi solves costly verification problems. Businesses spend time and resources checking if documents, credentials, and data are valid. DeDi creates a universal way to verify public information instantly.

> DeDi enables discovery of public keys, of up-to-date revocation lists, membership lists, sanctions list and other such positive/negative lists published by the sources of truth, making it easy to establish trust in the agentic AI world!

Every day, people carry out millions of digital transactions, which underpin the global economy. Trust, enabled by secure, interoperable, and efficient digital systems, serves as the foundation of this massive digital economy.

Establishing trust by verifying the integrity, validity, and authenticity of documents, digital credentials, transactions, or data packets is paramount for seamless execution of these transactions. While integrity can often be verified through digital signatures, ensuring validity (is the information current, not revoked?) and authenticity (is the source trustworthy?) remains a significant challenge. (For details on Integrity, Validity, and Authenticity, refer to ‘Understanding Trust Pillars’ in Appendix)

In our interconnected digital world, trust is established through public information maintained by custodians, registrars, and institutions of authority. These are often referred to as public directories – specialized electronic registries containing publicly available information about entities and things. Examples include directories of companies, banks, courts, language codes, location codes, directories of registered professionals (e.g., doctors, lawyers), a directory of government-authorized service providers, and more such directories. Historically, even "Yellow Pages" served as a form of public directory.

Currently, establishing this trust involves:

- **Inefficient Processes, leading to Higher Costs:**  Organizations spend excessive time and resources manually searching for and verifying critical information. These outdated methods can slow down operations, increase labor costs, and introduce errors that damage reliability.

- **Risk of Bad Decisions due to Delayed or Outdated Information:**  When up-to-date information isn’t readily available, businesses may miss important updates (like revoked credentials), potentially making wrong decisions or exposing themselves to fraud and security risks.

- **Difficult Integrations result in Missed Opportunities and Slower Growth:**  Inconsistent and custom-built interfaces make it hard for businesses to connect with multiple registries. This adds integration costs, delays time-to-market, and hinders innovation or expansion into new markets where interoperability is key.

DeDi overcomes these challenges by transforming fragmented and inefficient registries—whether manual or digital—into advanced, AI-ready directories. This empowers businesses with instant access to reliable information, streamlined processes, and seamless integration for the demands of the digital age. It provides a unified, machine-readable interface for accessing essential public information, fostering secure and efficient digital transactions.

## Decentralized Directory Protocol – universal way to discover & verify public information
DeDi unlocks new business value by making public directories accessible through a universal protocol coupled with suggested practices.

|                     **Business Value**                    |                   **How It Helps Your Business**                  |                           **Capability Enabling It**                           |
| :---------------------------------------------------: | :-----------------------------------------------------------: | :------------------------------------------------------------------------: |
|         Instantly access reliable information         | Make faster, more accurate business decisions with confidence |          Machine-Readable APIs: Standardized, programmable access          |
|      Know the source and history of every record      |         Simplify compliance and increase transparency         | Provenance Tracking & Audit Trails: Recorded authorship and change history |
|       Trust your data and protect against fraud       |             Strengthen security and customer trust            |       Cryptographic Tamper-Resistance: Immutable, verifiable records       |
|    Find and use the data you need, without barriers   |       Improve efficiency and serve more customers easily      |           Public, Interoperable Interfaces: Uniform, open access           |
|    Minimize costs and future-proof your operations    |   Stay agile and ready for growth with seamless integration   |   Compatibility Layers & Modular Integration: Works with current systems   |
| Always make decisions based on the latest information |       Reduce risk by avoiding outdated or incorrect data      |  Live Sync & Frequent Updates: Real-time or near-real-time record updates  |



The Decentralized Directory (DeDi) is an open specification for a unified interface to access public directories. It doesn't prescribe changes to existing processes but defines characteristics that make public directories suitable for enhancing trust at a low cost. A good public directory is (one or more of the following):

- **Publicly Accessible:** Allows anyone to look up specific records or query for multiple records, promoting transparency and broad usability.

- **Machine-Readable:** Accessible over standard APIs (HTTPS, RESTful) with well-known schemas for easy programmatic interpretation and automation.

- **Tamper-Resistant:** Offers cryptographic guarantees to ensure the immutability and verifiability of records, enabling a "trust, but verify" approach.
Provenance-Enabled: Provides authorship of entries and a history of changes, allowing access to earlier versions for transparency and auditability.

- **Live and Frequently Updated:** Ensures that information such as membership statuses, sanctions, and public keys is always up-to-date, reducing risks associated with outdated data.

## How Does DeDi Enable This?

At its core, DeDi is an open protocol that defines universal, standardized API specifications for accessing any public registry, enabling seamless lookup and querying across diverse information sources. By providing open-source API specifications, DeDi eliminates the need for costly custom integrations, allowing registrars to implement a unified and consistent interface for public information access.

DeDi’s information architecture is organized around three key constructs:

- **Namespace:** Corresponds to an organization (and implicitly to a domain name) as a starting point for trust.

- **Directory:** Refers to a list of records with configurable schemas.

- **Records:** The actual values or pointers to information.

> Important Note: The Decentralized Directory Protocol is not a software product or a rigid implementation manual—it is an open standard designed to enable trust in digital transactions by providing a universal, interoperable foundation for accessing and verifying public information.

## dedi.global – ready to use solution

To accelerate and simplify adoption, a ready-to-use hosted platform – dedi.global, is offered by the Network for Humanity Foundation. This philanthropic initiative allows registrars to effortlessly publish and manage their directories on a robust decentralized infrastructure, leveraging blockchain for automated governance, scalability, and enhanced trust—complementing and fully aligned with the open DeDi protocol.

By adopting the DeDi Protocol, relying parties can maintain a list of trusted domains and seamlessly query and look up public information, verifying digital assets and identities with unprecedented ease and confidence.

DeDi supports the co-existence of multiple data standards and schemas (e.g., VC JSON-LD, mDocs/mDL) and can output information in various formats, ensuring compatibility with existing systems and technologies. This makes it secure, trustworthy, and easy to use at scale, without disrupting your existing operations. More reosurces and API tools can be found [here](https://dedi-global.gitbook.io/docs) 

## Get Started

Your participation is crucial to the success of this initiative. Here's how you can help us build a more secure digital future:

- Claim Your Namespace: Register your organization on dedi.global. Publish your first directory!
- Adopt the DeDi Protocol to start looking up and querying public records in verification flows. Connect your verification systems to dedi.global.
- Implement the DeDi APIs in your existing public registries to make them accessible.

Let’s co-create a future where trust is seamlessly integrated into every digital transaction.

---

## Appendix: Three types of verification

In the digital world, establishing trust in any piece of information, whether it's a document, a transaction, or a digital credential, relies on three fundamental pillars: Integrity, Validity, and Authenticity.

### Integrity:
- _What it means:_ Integrity means no changes. The data matches the original exactly. Integrity check means to verify if the information has not been changed, altered, or tampered with since it was originally created or issued. It's about making sure the data is exactly as the sender intended it to be, without any unauthorized modifications.
- _How it's checked:_ This is often verified using digital signatures or cryptographic hash functions. A unique "fingerprint" of the data is created when it's issued. If even a single character is changed, the fingerprint will no longer match, indicating tampering.
- _Example:_ Imagine a digital degree certificate. If someone tries to change your grade from a 'B' to an 'A' after it's been issued, the integrity check would fail because the digital signature on the certificate would no longer be valid for the altered content.

### Validity:
- _What it means:_ Validity means still good. The information hasn't expired or been canceled. Validity check means to verify if the information is still current, relevant, and has not been revoked, expired, or superseded. It addresses whether the information is still "good" to use at the present time.
- _How it's checked:_ This typically involves checking against revocation lists (lists of certificates or credentials that have been cancelled), expiration dates, or updated versions of the information.
- _Example:_ A digital driver's license has an expiration date. Even if it's authentic and hasn't been tampered with, it becomes invalid after that date. Similarly, a digital certificate issued to a company might be revoked if the company goes out of business or its security is compromised.

### Authenticity:
- _What it means:_ Authenticity means real source. The information comes from who it claims to come from. Authenticity check means to verify if the source of the information is genuinely who or what it claims to be. It's about trusting the origin of the data.
- _How it's checked:_ This is established by verifying the digital signature against the issuer's publicly listed digital certificate, public key, or other trusted identifiers. It also involves checking if the issuer is a recognized and authorized entity (e.g., a university accredited to issue degrees, a bank licensed to operate).
- _Example:_ If you receive a digital invoice, authenticity ensures that it truly came from the company you did business with, not a fraudster pretending to be them. You would check the company's digital signature and verify that the signing certificate was issued by a trusted Certificate Authority and that the company is a legitimate, registered business.
