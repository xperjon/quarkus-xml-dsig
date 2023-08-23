# quarkus-xml-dsig
Example repo to reproduce that no debug logs are printed from org.jcp.xml.dsig.internal using Quarkus.

## Build

```
mvn clean package
```

## Run Quarkus

```
mvn quarkus:run

```
No debug logs from org.jcp.xml.dsig.internal will show:
```
...
2023-08-23 15:34:57,747 DEBUG [io.qua.dep.QuarkusAugmentor] (main) Quarkus augmentation completed in 3515ms
Signature failed core validation
signature validation status: false
ref[0] validity status: false
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
2023-08-23 15:34:58,038 DEBUG [io.qua.boo.cla.QuarkusClassLoader] (main) Adding elements io.quarkus.bootstrap.classloading.MemoryClassPathElement@400edeac to QuarkusClassLoader Quarkus Runtime ClassLoader: DEV restart no:0
...
```
## Run main class with plain java

```
cd target
java -Djava.util.logging.config.file=logging.properties org.acme.Main

```
Debug logs from org.jcp.xml.dsig.internal will show:

```
Aug 23, 2023 3:16:59 PM com.sun.org.apache.xml.internal.security.Init dynamicInit
FINE: Registering default algorithms
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignatureMethod verify
FINE: Signature provider: SunRsaSign version 17
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignatureMethod verify
FINE: Verifying with key: Sun RSA public key, 2048 bits
  params: null
  modulus: 31064805150737413760746821751759179188992818167461837521250593678755170243747146549525756639717647188956791610133697427958592369985479202058922071430085158528495658192924510128975107282271928073866201852705920090144112570726418365879239629689585113610295361335271184866314989336871561445785102147374996340641700840528358892187671019295725849448255260714402951632943480579965549068688882472829244202444212666402994608453122268094464756075908257417430153721856818381556895935683705156143884706692677030124626052072740227787709696986372196671088326362245809148809055679189382446973846067516698324312048317298705147172903
  public exponent: 65537
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignatureMethod verify
FINE: JCA Algorithm: SHA256withRSA
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignatureMethod verify
FINE: Signature Bytes length: 256
Aug 23, 2023 3:17:00 PM com.sun.org.apache.xml.internal.security.transforms.Transform initializeTransform
FINE: Create URI "http://www.w3.org/TR/2001/REC-xml-c14n-20010315#WithComments" class "class com.sun.org.apache.xml.internal.security.transforms.implementations.TransformC14NWithComments"
Aug 23, 2023 3:17:00 PM com.sun.org.apache.xml.internal.security.transforms.Transform <init>
FINE: The NodeList is [CanonicalizationMethod: null]
Aug 23, 2023 3:17:00 PM com.sun.org.apache.xml.internal.security.utils.ElementProxy setElement
FINE: setElement(CanonicalizationMethod, "null")
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.ApacheCanonicalizer transform
FINE: Created transform for algorithm: http://www.w3.org/TR/2001/REC-xml-c14n-20010315#WithComments
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.ApacheCanonicalizer transform
FINE: isNodeSet() = true
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignedInfo canonicalize
FINE: Canonicalized SignedInfo:
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignedInfo canonicalize
FINE: <SignedInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
      <CanonicalizationMethod Algorithm="http://www.w3.org/TR/2001/REC-xml-c14n-20010315#WithComments"></CanonicalizationMethod>
      <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"></SignatureMethod>
      <Reference URI="">
        <Transforms>
          <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"></Transform>
        </Transforms>
        <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"></DigestMethod>
        <DigestValue>/juoQ4bDxElf1M+KJauO20euW+QAvvPP0nDCruCQooM=</DigestValue>
      </Reference>
    </SignedInfo>
Aug 23, 2023 3:17:00 PM org.jcp.xml.dsig.internal.dom.DOMSignedInfo canonicalize
FINE: Data to be signed/verified:PFNpZ25lZEluZm8geG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvMDkveG1sZHNpZyMiPgog
ICAgICA8Q2Fub25pY2FsaXphdGlvbk1ldGhvZCBBbGdvcml0aG09Imh0dHA6Ly93d3cudzMub3Jn
L1RSLzIwMDEvUkVDLXhtbC1jMTRuLTIwMDEwMzE1I1dpdGhDb21tZW50cyI+PC9DYW5vbmljYWxp
emF0aW9uTWV0aG9kPgogICAgICA8U2lnbmF0dXJlTWV0aG9kIEFsZ29yaXRobT0iaHR0cDovL3d3
dy53My5vcmcvMjAwMS8wNC94bWxkc2lnLW1vcmUjcnNhLXNoYTI1NiI+PC9TaWduYXR1cmVNZXRo
b2Q+CiAgICAgIDxSZWZlcmVuY2UgVVJJPSIiPgogICAgICAgIDxUcmFuc2Zvcm1zPgogICAgICAg
ICAgPFRyYW5zZm9ybSBBbGdvcml0aG09Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvMDkveG1sZHNp
ZyNlbnZlbG9wZWQtc2lnbmF0dXJlIj48L1RyYW5zZm9ybT4KICAgICAgICA8L1RyYW5zZm9ybXM+
CiAgICAgICAgPERpZ2VzdE1ldGhvZCBBbGdvcml0aG09Imh0dHA6Ly93d3cudzMub3JnLzIwMDEv
MDQveG1sZW5jI3NoYTI1NiI+PC9EaWdlc3RNZXRob2Q+CiAgICAgICAgPERpZ2VzdFZhbHVlPi9q
dW9RNGJEeEVsZjFNK0tKYXVPMjBldVcrUUF2dlBQMG5EQ3J1Q1Fvb009PC9EaWdlc3RWYWx1ZT4K
ICAgICAgPC9SZWZlcmVuY2U+CiAgICA8L1NpZ25lZEluZm8+
Signature failed core validation
signature validation status: false
...
```
