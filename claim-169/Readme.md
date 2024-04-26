# CBOR Identity Data in QR-Code

**Tag**: 169 (identity-data)

**Data Item**: JSON Object

**Semantics**: Unique Identity Data of a Citizen in QR-Code

**Point of contact**: Resham Chugani (resham@mosip.io)

# 1.Introduction

This document specifies a generic data structure and encoding mechanisms for storing Unique Identity Data of a Citizen registered using any ID platforms. It also specifies a transport encoding mechanism in a machine-readable optical format (QR).

# 2.Rationale

Once a citizen is registered in an identity system, their data serves as the foundation for identification, granting them access to social benefits and government services. The level of assurance in this identification process varies depending on the authentication method employed. Low assurance is achieved through the use of basic identifiers like ID numbers, demographic data, passwords, or PINs. Conversely, higher assurance levels are attained through methods such as one-time passwords (OTP) or biometrics.

Among these methods, biometric based authentication, such as facial authentication, offers the highest level of assurance as it directly verifies the presence of the citizen. While this is effective for online systems where verification is conducted on a server, offline authentication presents challenges in maintaining a similarly high level of assurance and also works for people with no phone..

For instance, in scenarios involving cross-border verification, remote areas often face significant internet connectivity issues. Even when internet access is available, server reliability may be inconsistent. In such circumstances, scanning a QR code containing a citizen's facial photograph and identity information, alongside assurance that the data is country-signed, provides an additional layer of security and affirmation for the countries involved.

To tackle the aforementioned challenge and include a face photograph within the QR code, we propose a solution that involves embedding a low-resolution grayscale image of the citizen along with a minimal demographic dataset within the QR code. This QR code would be digitally signed by the ID system and then printed on a physical card. Subsequently, the signed data within the QR code can be utilized for facial authentication. However, it's essential to recognize that QR codes have limitations regarding size. To address this, we suggest leveraging CBOR Web Token (CWT) to generate a smaller signature and more condensed data.

# 3.Semantics

Claim 169 represents a JSON Object that includes a range of ID attributes defined by the issuing identity system as key-value pairs. Below, you can find an illustration of the ID JSON structure contained within Claim 169, where:
### 3.1 JSON Structure Overview

| Attribute | Type  | Attribute Name       | Description                                                                                                                                                                                                                     |
|-----------|-------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1         | tstr  | ID                   | Unique ID to indicate the PII data                                                                                                                                                                                              |
| 2         | tstr  | Version              | Version of the ID data                                                                                                                                                                                                          |
| 3         | tstr  | Language             | Language of citizen                                                                                                                                                                                                             |
| 4         | tstr  | Full Name            | Full name of the citizen                                                                                                                                                                                                        |
| 5         | tstr  | First Name           | First name of the citizen                                                                                                                                                                                                       |
| 6         | tstr  | Middle Name          | Middle name of the citizen                                                                                                                                                                                                      |
| 7         | tstr  | Last Name            | Last name of the citizen                                                                                                                                                                                                        |
| 8         | tstr  | Date of Birth        | Date of birth of the citizen. Must be in YYYYMMDD format                                                                                                                                                                        |
| 9         | int   | Gender               | Gender of the citizen. Can contain following values 1 - Male, 2 - Female, 3 - Others                                                                                                                                            |
| 10        | tstr  | Address              | Address of the citizen with address line separator character `\n`                                                                                                                                                               |
| 11        | tstr  | Email ID             | Email id of the citizen                                                                                                                                                                                                         |
| 12        | tstr  | Phone Number         | Contact number of the citizen                                                                                                                                                                                                   |
| 13        | tstr  | Nationality          | Nationality of the citizen                                                                                                                                                                                                      |
| 14        | int   | Marital Status       | Marital status of the citizen.Can contain following values 1 - Unmarried, 2 - Married, 3 - Divorced                                                                                                                             |
| 15        | tstr  | Guardian             | Name of the entity playing the role of a guardian of the citizen such as mother, father, spouse, sister, legal guardian etc.                                                                                                    |
| 16        | tstr  | Binary Image         | Binary image data representing the citizen's photograph                                                                                                                                                                         |
| 17        | int   | Binary Image Format  | Binary image format of the binary image data. Can contain following values 1 - JPEG, 2 - JPEG2, 3 - AVIF, 4- WEBP                                                                                                               |
| 18        | [int] | Best Quality Fingers | An unsigned 8-bit number encoding hand position of the finger. It must be in range 0-10, where 0 represents "Unknown", 1-5 represents right thumb to little finger, and 6-10 represents left thumb to little finger in sequence |
| 19        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 20        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 21        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 22        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 23        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 24        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 25        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 26        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 27        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 28        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 29        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 30        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 31        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 32        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 33        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 34        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 35        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 36        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 37        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 38        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 39        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 40        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 41        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 42        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 43        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 44        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 45        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 46        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 47        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 48        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 49        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 50        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 51        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 52        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 53        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 54        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 55        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 56        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 57        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 58        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 59        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 60        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 61        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 62        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 63        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 64        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 65        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 66        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 67        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 68        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 69        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 70        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 71        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 72        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 73        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 74        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 75        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 76        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 77        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 78        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 79        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 80        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 81        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 82        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 83        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 84        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 85        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 86        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 87        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 88        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 89        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 90        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 91        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 92        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 93        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 94        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 95        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 96        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 97        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 98        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |
| 99        | tstr  |                      | Reserved for future attributes                                                                                                                                                                                                  |

### 3.2 JSON Structure Example
```CWT
{
   "1":"COUN",
   "6":1665980929,
   "8":{
      "3":"dfd1aa976d8d4575a0fe34b96de2bfad"
   },
   "169":{
        "1": "11110000324013",
        "2": "1.0",
        "3": "EN",
        "4": "Peter M Jhon",
        "5": "Peter",
        "6": "M",
        "7": "Jhon",
        "8": "19880102",
        "9": 1,
        "10": "New City, METRO LINE, PA",
        "11": "peter@example.com",
        "12": "+1 234-567",
        "13": "US",
        "14": 2,
        "15": "Jhon Honai",
        "16": "03CBABDF83D068ACB5DE65B3CDF25E0036F2C546CB90378C587A076E7A759DFD27CA7872B6CDFF339AEAACA61A6023FD1D305A9B4F33CAA248CEDE38B67D7C915C59A51BB4E77D10077A625258873183F82D65F4C482503A5A01F41DEE612C3542E5370987815E592B8EA2020FD3BDDC747897DB10237EAD179E55B441BC6D8BAD07CE535129CF8D559445CC3A29D746FBF1174DE2E7C0F3439BE7DBEA4520CF88825AAE6B1F291A746AB8177C65B2A459DD19BD32C0C3070004B85C1D63034707CC690AB0BA023350C8337FC6894061EB8A714A8F22FE2365E7A904C72DEC9746ABEA1A3296ECACD1A40450794EDCD2B34844E7C19EB7FB1A4AF3B05C3B374BD2941603F72D3F9A62EAB9A2FDAEEEEC8EE6E350F8A1863C0A0AB1B4058D154559A1CD5133EFCF682ABC339960819C9427889D60380B635A7D21D017974BBA57798490F668ADD86DA58125D9C4C1202CA1308F7734E43E8F77CEB0AF968A8F8B88849F9B98B26620399470ED057E7931DED82876DCA896A30D0031A8CBD7B9EDFDF16C15C6853F4F8D9EEC09317C84EDAE4B349FE54D23D8EC7DC9BB9F69FD7B7B23383B64F22E25F",
        "17": 2,
        "18": [1,2],
}
```

### 4. CBOR Identity Data in QR-Code Claims Registration
This specification registers the `identity-data` claim in the IANA "CBOR Web Token (CWT) Claims" registry [IANA.CWT.Claims], established by [RFC8392].

#### 4.1 Registry Contents
Claim Name: identity-data

Claim Description: Registering the claim for storing unique identity data of a citizen, which could be personally identifiable data (PII), using any ID platform. 

Claim Key: 169

Claim Value Type(s): map

Change Controller: MOSIP

Specification Document(s): [Section 1](#1introduction)


# References

[1] C. Bormann, and P. Hoffman. "Concise Binary Object Representation (CBOR)". [RFC 7049](https://tools.ietf.org/html/rfc7049), October 2013.

[2] Mike Jones, Hannes Tschofenig, Ludwig Seitz  "CBOR Web Token (CWT)". [RFC 8392](https://www.iana.org/go/rfc8392), March 2018.


# Author

Resham Chugani ([resham@mosip.io](mailto:resham@mosip.io))
