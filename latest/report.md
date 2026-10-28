# JDK eMudhra Security Audit

Generated (UTC): `2026-10-28T14:26:18+00:00`

SDKMAN supplies vendor names only; the audit runs Docker JDK 25 images. Static file checks, trust-manager checks, and live TLS tests are independent. Image tags can change between runs.

## Column guide

- **Vendor:** SDKMAN provider name used to select the Docker image; SDKMAN binaries are not tested.
- **JDK:** Requested Java major version (currently 25).
- **Docker image:** Exact image tag tested; links to the vendor image repository, not the scanned digest.
- **Runtime:** Java version and JVM os.arch architecture collected inside the image; the report JSON retains its image digest.
- **blocked.certs:** Number of the 48 expected compromised-certificate SHA-256 fingerprints present in the JDK blocklist. This is a static file check.
- **CheckCerts (CVE-2026-114820):** Runs the OpenJDK CheckCerts utility with the effective default X509TrustManager. ROOTS_ABSENT means none of the four monitored root CAs appears among the effective default JVM trust manager's accepted issuers (desired root-removal inventory result). ROOT_PRESENT means one or more monitored roots is still listed; the linked log identifies them. These are root-inventory results, not TLS handshake verdicts. Relates to the root-removal remediation for CVE-2026-114820, but is not a live TLS-chain test. Advisory: https://www.oracle.com/security-alerts/alert-cve-2026-114820.html.
- **TLS chain test:** Live Java HTTPS test of 12 endpoints; verifies pinned compromised intermediate public keys. PASS means all 12 rejected; FAIL means a verified compromised chain was accepted; INCONCLUSIVE means insufficient evidence.

## Check methodology

### blocked.certs fingerprint inventory

Checks the presence of the 48 expected compromised-intermediate fingerprints; it does not establish runtime TLS behavior.

[Official/reference documentation](https://ops.java/emudhra/)

### CheckCerts (CVE-2026-114820)

Runs the original OpenJDK source against the stock JDK default TrustManagerFactory, inspecting accepted issuers for four monitored roots. ROOTS_ABSENT means the effective default JVM trust manager does not list any of the four monitored roots (desired inventory result); ROOT_PRESENT means at least one is listed and its subject is recorded in the linked log. Neither result alone determines whether compromised TLS chains are accepted. This is an effective root-inventory check related to Oracle root-removal remediation for CVE-2026-114820, not a TLS handshake test.

[Official/reference documentation](https://www.oracle.com/security-alerts/alert-cve-2026-114820.html)

### TLS chain test

Uses a delegating Java trust manager to check twelve live certificate chains with pinned compromised intermediate SPKI hashes; PASS indicates all twelve rejected. Not a universal proof of PKIX immunity.

[Official/reference documentation](https://docs.oracle.com/en/java/javase/25/docs/api/java.net.http/java/net/http/HttpClient.Builder.html)


| Vendor | JDK | Docker image | Runtime | blocked.certs | CheckCerts (CVE-2026-114820) | TLS chain test |
| --- | --- | --- | --- | --- | --- | --- |
| Corretto | 25 | [amazoncorretto:25](https://hub.docker.com/_/amazoncorretto) | openjdk version "25.0.5" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Corretto-jdk25/checkcerts.log) | [PASS (12/12 rejected)](evidence/Corretto-jdk25/compromisedca.log) |
| GraalVM CE | 25 | [ghcr.io/graalvm/jdk-community:25](https://github.com/graalvm/jdk-community) | openjdk version "25.0.2" 2026-01-20 [amd64] | 0/48 | [ROOT_PRESENT](evidence/GraalVM_CE-jdk25/checkcerts.log) | [FAIL](evidence/GraalVM_CE-jdk25/compromisedca.log) |
| GraalVM Oracle | 25 | [container-registry.oracle.com/graalvm/jdk:25](https://container-registry.oracle.com/graalvm/jdk) | java version "25.0.5.0.3" 2026-10-25 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/GraalVM_Oracle-jdk25/checkcerts.log) | [PASS (12/12 rejected)](evidence/GraalVM_Oracle-jdk25/compromisedca.log) |
| Liberica | 25 | [bellsoft/liberica-openjdk-debian:25](https://hub.docker.com/r/bellsoft/liberica-openjdk-debian) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Liberica-jdk25/checkcerts.log) | [FAIL](evidence/Liberica-jdk25/compromisedca.log) |
| Liberica NIK | 25 | [bellsoft/hardened-liberica-native-image-kit-container:jdk-25-nik-25-glibc](https://hub.docker.com/r/bellsoft/hardened-liberica-native-image-kit-container) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Liberica_NIK-jdk25/checkcerts.log) | [FAIL](evidence/Liberica_NIK-jdk25/compromisedca.log) |
| Mandrel | 25 | [quay.io/quarkus/ubi9-quarkus-mandrel-builder-image:jdk-25](https://quay.io/quarkus/ubi9-quarkus-mandrel-builder-image) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Mandrel-jdk25/checkcerts.log) | [PASS (12/12 rejected)](evidence/Mandrel-jdk25/compromisedca.log) |
| Microsoft | 25 | [mcr.microsoft.com/openjdk/jdk:25-ubuntu](https://mcr.microsoft.com/en-us/artifact/mar/openjdk/jdk/tags) | openjdk version "25.0.5.0.2" 2026-10-22 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Microsoft-jdk25/checkcerts.log) | [PASS (12/12 rejected)](evidence/Microsoft-jdk25/compromisedca.log) |
| SapMachine | 25 | [ghcr.io/sap/sapmachine:25](https://github.com/sap/sapmachine) | openjdk version "25.0.5.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/SapMachine-jdk25/checkcerts.log) | [PASS (12/12 rejected)](evidence/SapMachine-jdk25/compromisedca.log) |
| Semeru | 25 | [icr.io/appcafe/ibm-semeru-runtimes:open-25-jdk-ubi](https://icr.io/appcafe/ibm-semeru-runtimes) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Semeru-jdk25/checkcerts.log) | [FAIL](evidence/Semeru-jdk25/compromisedca.log) |
| Temurin | 25 | [eclipse-temurin:25-jdk](https://hub.docker.com/_/eclipse-temurin) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Temurin-jdk25/checkcerts.log) | [FAIL](evidence/Temurin-jdk25/compromisedca.log) |
| Zulu | 25 | [azul-zulu:25](https://hub.docker.com/_/azul-zulu) | openjdk version "25.0.5" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Zulu-jdk25/checkcerts.log) | [PASS (12/12 rejected)](evidence/Zulu-jdk25/compromisedca.log) |

[Download everything — complete report and vendor evidence (ZIP)](report-bundle.zip)

**TLS outcomes:** PASS=6, FAIL=5, INCONCLUSIVE=0, NOT_TESTED=0

Static file checks cannot by themselves establish functional TLS protection.
Static checks inspect bundled files; TLS chain test separately runs live JSSE tests against pinned intermediate SPKIs.


CheckCerts is a root inventory; TLS chain test is a live certificate-chain test. Static classifications remain in report.json.
