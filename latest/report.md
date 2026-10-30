# JDK eMudhra Security Audit

Generated (UTC): `2026-10-30T09:53:44+00:00`

SDKMAN supplies vendor names only; the audit runs Docker JDK 25 images. Static file checks, trust-manager checks, and live TLS tests are independent. Image tags can change between runs.

## Column guide

- **Vendor:** SDKMAN provider name used to select the Docker image; SDKMAN binaries are not tested.
- **JDK / Docker image:** Requested Java major version (17, 21, or 25) and linked tested Docker image tag. The scanned image digest remains available in JSON.
- **Runtime:** Java version and JVM os.arch architecture collected inside the image; the report JSON retains its image digest.
- **blocked certs:** Number of the 48 expected compromised-certificate SHA-256 fingerprints present in the JDK blocklist. This is a static file check.
- **CheckCerts:** Checks whether the JDK still trusts any of the four root certificates Oracle recommends removing for CVE-2026-114820. ROOTS_ABSENT means none of these certificates is trusted by default. ROOT_PRESENT means at least one is still trusted; the linked log shows which ones. The check uses Java's default trust manager rather than inspecting certificate files directly. It does not test whether the JDK accepts compromised TLS certificate chains; that is covered by the separate TLS chain test.
- **TLS chain test:** Live Java HTTPS test of 12 endpoints; verifies pinned compromised intermediate public keys. PASS means all 12 rejected; FAIL means a verified compromised chain was accepted; INCONCLUSIVE means insufficient evidence.

## Check methodology

### blocked.certs fingerprint inventory (CVE-2026-91403)

Checks whether the JDK blocked.certs file contains all 48 expected SHA-256 fingerprints associated with the compromised eMudhra intermediate certificates. This is a static inventory check related to Oracle CVE-2026-91403; it does not validate TLS behavior or establish complete mitigation. OpenJDK subsequently advised root CA removal under CVE-2026-114820 because intermediate-only blocking was insufficient.

[Official/reference documentation](https://www.oracle.com/security-alerts/alert-cve-2026-91403.html)

### CheckCerts (CVE-2026-114820)

Checks whether the JDK still trusts any of the four root certificates Oracle recommends removing for CVE-2026-114820. ROOTS_ABSENT means none of these certificates is trusted by default. ROOT_PRESENT means at least one is still trusted; the linked log shows which ones. The check uses Java's default trust manager rather than inspecting certificate files directly. It does not test whether the JDK accepts compromised TLS certificate chains; that is covered by the separate TLS chain test.

[Official/reference documentation](https://github.com/advisories/GHSA-255q-pwv4-fjj7)

### TLS chain test

Uses a delegating Java trust manager to check twelve live certificate chains with pinned compromised intermediate SPKI hashes; PASS indicates all twelve rejected. Not a universal proof of PKIX immunity.

[Official/reference documentation](https://docs.oracle.com/en/java/javase/25/docs/api/java.net.http/java/net/http/HttpClient.Builder.html)


| Vendor | JDK / Docker image | Runtime | blocked certs | CheckCerts | TLS chain test |
| --- | --- | --- | --- | --- | --- |
| Corretto | 17 - [amazoncorretto:17](https://hub.docker.com/_/amazoncorretto) | openjdk version "17.0.21" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Corretto-jdk17/checkcerts.log) | [PASS (12/12)](evidence/Corretto-jdk17/compromisedca.log) |
| Corretto | 21 - [amazoncorretto:21](https://hub.docker.com/_/amazoncorretto) | openjdk version "21.0.13" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Corretto-jdk21/checkcerts.log) | [PASS (12/12)](evidence/Corretto-jdk21/compromisedca.log) |
| Corretto | 25 - [amazoncorretto:25](https://hub.docker.com/_/amazoncorretto) | openjdk version "25.0.5" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Corretto-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Corretto-jdk25/compromisedca.log) |
| GraalVM CE | 17 - [ghcr.io/graalvm/jdk-community:17](https://github.com/graalvm/jdk-community) | openjdk version "17.0.9" 2023-10-17 [amd64] | 0/48 | [ROOT_PRESENT](evidence/GraalVM_CE-jdk17/checkcerts.log) | [FAIL](evidence/GraalVM_CE-jdk17/compromisedca.log) |
| GraalVM CE | 25 - [ghcr.io/graalvm/jdk-community:25](https://github.com/graalvm/jdk-community) | openjdk version "25.0.2" 2026-01-20 [amd64] | 0/48 | [ROOT_PRESENT](evidence/GraalVM_CE-jdk25/checkcerts.log) | [FAIL](evidence/GraalVM_CE-jdk25/compromisedca.log) |
| GraalVM Oracle | 17 - [container-registry.oracle.com/graalvm/jdk:17](https://container-registry.oracle.com/graalvm/jdk) | java version "17.0.16" 2025-07-15 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/GraalVM_Oracle-jdk17/checkcerts.log) | [FAIL](evidence/GraalVM_Oracle-jdk17/compromisedca.log) |
| GraalVM Oracle | 25 - [container-registry.oracle.com/graalvm/jdk:25](https://container-registry.oracle.com/graalvm/jdk) | java version "25.0.5.0.3" 2026-10-25 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/GraalVM_Oracle-jdk25/checkcerts.log) | [PASS (12/12)](evidence/GraalVM_Oracle-jdk25/compromisedca.log) |
| Huawei | 17 - [openeuler/bisheng-jdk:17.0.10-oe2203sp3](https://hub.docker.com/r/openeuler/bisheng-jdk) | openjdk version "17.0.10" 2024-01-16 [amd64] | 0/48 | [ROOT_PRESENT](evidence/Huawei-jdk17/checkcerts.log) | [FAIL](evidence/Huawei-jdk17/compromisedca.log) |
| Liberica | 17 - [bellsoft/liberica-openjdk-debian:17](https://hub.docker.com/r/bellsoft/liberica-openjdk-debian) | openjdk version "17.0.21.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/Liberica-jdk17/checkcerts.log) | [PASS (12/12)](evidence/Liberica-jdk17/compromisedca.log) |
| Liberica | 21 - [bellsoft/liberica-openjdk-debian:21](https://hub.docker.com/r/bellsoft/liberica-openjdk-debian) | openjdk version "21.0.13.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/Liberica-jdk21/checkcerts.log) | [PASS (12/12)](evidence/Liberica-jdk21/compromisedca.log) |
| Liberica | 25 - [bellsoft/liberica-openjdk-debian:25](https://hub.docker.com/r/bellsoft/liberica-openjdk-debian) | openjdk version "25.0.5.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/Liberica-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Liberica-jdk25/compromisedca.log) |
| Liberica NIK | 17 - [bellsoft/hardened-liberica-native-image-kit-container:jdk-17-nik-23-glibc](https://hub.docker.com/r/bellsoft/hardened-liberica-native-image-kit-container) | openjdk version "17.0.20.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Liberica_NIK-jdk17/checkcerts.log) | [FAIL](evidence/Liberica_NIK-jdk17/compromisedca.log) |
| Liberica NIK | 25 - [bellsoft/hardened-liberica-native-image-kit-container:jdk-25-nik-25-glibc](https://hub.docker.com/r/bellsoft/hardened-liberica-native-image-kit-container) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Liberica_NIK-jdk25/checkcerts.log) | [FAIL](evidence/Liberica_NIK-jdk25/compromisedca.log) |
| Mandrel | 25 - [quay.io/quarkus/ubi9-quarkus-mandrel-builder-image:jdk-25](https://quay.io/quarkus/ubi9-quarkus-mandrel-builder-image) | openjdk version "25.0.4.1" 2026-08-18 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Mandrel-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Mandrel-jdk25/compromisedca.log) |
| Microsoft | 17 - [mcr.microsoft.com/openjdk/jdk:17-ubuntu](https://mcr.microsoft.com/en-us/artifact/mar/openjdk/jdk/tags) | openjdk version "17.0.21.0.2" 2026-10-22 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Microsoft-jdk17/checkcerts.log) | [PASS (12/12)](evidence/Microsoft-jdk17/compromisedca.log) |
| Microsoft | 21 - [mcr.microsoft.com/openjdk/jdk:21-ubuntu](https://mcr.microsoft.com/en-us/artifact/mar/openjdk/jdk/tags) | openjdk version "21.0.13.0.2" 2026-10-22 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Microsoft-jdk21/checkcerts.log) | [PASS (12/12)](evidence/Microsoft-jdk21/compromisedca.log) |
| Microsoft | 25 - [mcr.microsoft.com/openjdk/jdk:25-ubuntu](https://mcr.microsoft.com/en-us/artifact/mar/openjdk/jdk/tags) | openjdk version "25.0.5.0.2" 2026-10-22 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Microsoft-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Microsoft-jdk25/compromisedca.log) |
| SapMachine | 17 - [ghcr.io/sap/sapmachine:17](https://github.com/sap/sapmachine) | openjdk version "17.0.21.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/SapMachine-jdk17/checkcerts.log) | [PASS (12/12)](evidence/SapMachine-jdk17/compromisedca.log) |
| SapMachine | 21 - [ghcr.io/sap/sapmachine:21](https://github.com/sap/sapmachine) | openjdk version "21.0.13.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/SapMachine-jdk21/checkcerts.log) | [PASS (12/12)](evidence/SapMachine-jdk21/compromisedca.log) |
| SapMachine | 25 - [ghcr.io/sap/sapmachine:25](https://github.com/sap/sapmachine) | openjdk version "25.0.5.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOTS_ABSENT](evidence/SapMachine-jdk25/checkcerts.log) | [PASS (12/12)](evidence/SapMachine-jdk25/compromisedca.log) |
| Semeru | 17 - [icr.io/appcafe/ibm-semeru-runtimes:open-17-jdk-ubi](https://icr.io/appcafe/ibm-semeru-runtimes) | openjdk version "17.0.21.0.3" 2026-10-24 [amd64] | 48/48 | [ROOT_PRESENT](evidence/Semeru-jdk17/checkcerts.log) | [PASS (12/12)](evidence/Semeru-jdk17/compromisedca.log) |
| Semeru | 21 - [icr.io/appcafe/ibm-semeru-runtimes:open-21-jdk-ubi](https://icr.io/appcafe/ibm-semeru-runtimes) | openjdk version "21.0.13.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Semeru-jdk21/checkcerts.log) | [PASS (12/12)](evidence/Semeru-jdk21/compromisedca.log) |
| Semeru | 25 - [icr.io/appcafe/ibm-semeru-runtimes:open-25-jdk-ubi](https://icr.io/appcafe/ibm-semeru-runtimes) | openjdk version "25.0.5.0.3" 2026-10-24 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Semeru-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Semeru-jdk25/compromisedca.log) |
| Temurin | 17 - [eclipse-temurin:17-jdk](https://hub.docker.com/_/eclipse-temurin) | openjdk version "17.0.20.1" 2026-08-18 [amd64] | 0/48 | [ROOT_PRESENT](evidence/Temurin-jdk17/checkcerts.log) | [FAIL](evidence/Temurin-jdk17/compromisedca.log) |
| Temurin | 21 - [eclipse-temurin:21-jdk](https://hub.docker.com/_/eclipse-temurin) | openjdk version "21.0.13" 2026-10-20 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Temurin-jdk21/checkcerts.log) | [PASS (12/12)](evidence/Temurin-jdk21/compromisedca.log) |
| Temurin | 25 - [eclipse-temurin:25-jdk](https://hub.docker.com/_/eclipse-temurin) | openjdk version "25.0.5" 2026-10-20 LTS [amd64] | 0/48 | [ROOT_PRESENT](evidence/Temurin-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Temurin-jdk25/compromisedca.log) |
| Zulu | 17 - [azul-zulu:17](https://hub.docker.com/_/azul-zulu) | openjdk version "17.0.21" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Zulu-jdk17/checkcerts.log) | [PASS (12/12)](evidence/Zulu-jdk17/compromisedca.log) |
| Zulu | 21 - [azul-zulu:21](https://hub.docker.com/_/azul-zulu) | openjdk version "21.0.13" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Zulu-jdk21/checkcerts.log) | [PASS (12/12)](evidence/Zulu-jdk21/compromisedca.log) |
| Zulu | 25 - [azul-zulu:25](https://hub.docker.com/_/azul-zulu) | openjdk version "25.0.5" 2026-10-20 LTS [amd64] | 48/48 | [ROOT_PRESENT](evidence/Zulu-jdk25/checkcerts.log) | [PASS (12/12)](evidence/Zulu-jdk25/compromisedca.log) |

[Download everything — complete report and vendor evidence (ZIP)](report-bundle.zip)
