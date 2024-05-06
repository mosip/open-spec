# CBOR Identity Data in QR-Code

**Tag**: 169 (identity-data)

**Data Item**: JSON Object

**Semantics**: Identity Data of a Person in QR-Code

**Point of contact**: Resham Chugani (resham@mosip.io)

# 1.Introduction

This document specifies a generic data structure and encoding mechanism for storing the Identity Data of a registered person using any ID platform. It also provides a transport encoding mechanism in a machine-readable optical format (QR).

# 2.Rationale

Once a person is registered in an identity system, their data serves as the foundation for identification, granting them access to social benefits and government services. The level of assurance in this identification process varies depending on the authentication methods employed. Low assurance is achieved through basic identifiers like ID numbers, demographic data, passwords, or PINs. Conversely, higher assurance levels are attained through one-time passwords (OTP) and biometrics.

Among these methods, biometric-based authentication, such as facial authentication, offers the highest level of assurance as it assures the presence of the individual. While this is effective for online systems & personal phones where verification is conducted on a server or a personal device; offline authentication presents challenges in maintaining a similarly high level of assurance. The offline authentication mechanism should work for people with no phone.

For instance, in a cross-border scenario remote areas often face significant internet connectivity issues. Even when internet access is available, server reliability may be inconsistent. In such circumstances, scanning a QR code containing the person's facial photograph and identity information, alongside assurance that the data is country-signed, provides an additional layer of security and affirmation for the countries involved.

**Please note:** The trust layers required to sync the country's key are beyond the scope of this document. We assume the app scanning the QR code already has the country's key to verify.

To tackle the challenge above, we propose a standard CBOR-based QR Code that involves embedding a low-resolution image of the person with a minimal demographic dataset within the QR code. This QR code would be digitally signed by the ID authorities (Issuer) and then printed on a physical card. Subsequently, the signed data within the QR code can be utilized for facial authentication. However, it's essential to recognize that QR codes have limitations regarding size. We suggest leveraging CBOR Web Token (CWT) with ED25519/ECC keys to generate a smaller signature and more condensed data.

# 3.Semantics

Claim 169 represents a JSON Object that includes the below table as ID attributes. You can find an illustration of the ID structure contained within Claim 169, where:

## 3.1 CBOR Map Structure Overview

**All the fields here are Optional**

| Attribute | Type  | Attribute Name       | Description                                   |
|-----------|-------|----------------------|-----------------------------------------------|
| 1         | tstr  | ID                   | Unique ID to indicate the PII data            |
| 2         | tstr  | Version              | Version of the ID data                        |
| 3         | tstr  | Language             | Language used in other attributes             |
| 4         | tstr  | Full Name            | Full name of the  person                      |
| 5         | tstr  | First Name           | First name of the person                      |
| 6         | tstr  | Middle Name          | Middle name of the person                     |
| 7         | tstr  | Last Name            | Last name of the person                       |
| 8         | tstr  | Date of Birth        | Date of birth in YYYYMMDD format              |
| 9         | int   | Gender               | Gender with the following values 1 - Male, 2 - Female, 3 - Others                                                                                 |
| 10        | tstr  | Address              | Address of the person separator character `\n`|
| 11        | tstr  | Email ID             | Email id of the person                        |
| 12        | tstr  | Phone Number         | Contact number of the person                  |
| 13        | tstr  | Nationality          | Nationality of the person                     |
| 14        | int   | Marital Status       | Marital status - Can contain the following values 1 - Unmarried, 2 - Married, 3 - Divorced                                                       |
| 15        | tstr  | Guardian             | Name/id of the entity playing the role of a guardian, such as a mother, father, spouse, sister, legal guardian etc.                              |
| 16        | tstr  | Binary Image         | Binary image of the person's photograph       |
| 17        | int   | Binary Image Format  | Binary image format. Can contain the following values 1 - JPEG, 2 - JPEG2, 3 - AVIF, 4- WEBP                                                     |
| 18        | [int] | Best Quality Fingers | An unsigned 8-bit number encoding the hand position of the finger. It must be in the range 0-10, where 0 represents "Unknown", 1-5 represents right thumb to little finger, and 6-10 represents left thumb to little finger in sequence              |
| 19.. 99   | tstr  | Reserved             | Reserved for future attributes                |


## 3.2 CBOR Map Structure Example
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

# 4. CBOR Identity Data in QR-Code Claims Registration
This specification registers the `identity-data` claim in the IANA "CBOR Web Token (CWT) Claims" registry [IANA.CWT.Claims], established by [RFC8392](https://www.iana.org/go/rfc8392).

## 4.1 Registry Contents
Claim Name: identity-data

Claim Description: Registering the claim for storing identity data of a person, which could be personally identifiable data (PII) mostly used in Foundational/National ID for cross-border interoperability.

Claim Key: 169

Claim Value Type(s): map

Change Controller: MOSIP

Specification Document(s): [Section 1](#1introduction)


# References

[1] C. Bormann, and P. Hoffman. "Concise Binary Object Representation (CBOR)". [RFC 7049](https://tools.ietf.org/html/rfc7049), October 2013.

[2] Mike Jones, Hannes Tschofenig, Ludwig Seitz  "CBOR Web Token (CWT)". [RFC 8392](https://www.iana.org/go/rfc8392), March 2018.


# Author

Resham Chugani ([resham@mosip.io](mailto:resham@mosip.io))

Mahammed Taheer ([mohd.taheer@gmail.com](mailto:mohd.taheer@gmail.com))

Sasikumar G ([sasi@duck.com](mailto:sasi@duck.com))

Sreenadh S ([sreeavtar@gmail.com](mailto:sreeavtar@gmail.com))

Rounak Nayak ([rounak@ooru.io](mailto:rounak@ooru.io))
