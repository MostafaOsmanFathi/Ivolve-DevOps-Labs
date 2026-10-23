# Lab 5 - Image Vulnerability Scanning with Trivy

## Overview

Scan a Node.js image with Trivy, then fix the findings by updating the base image. The old base image was `node:18-bullseye` (Debian 11.11); the new one is `node:24-trixie-slim` (Debian 13.7).

## Proof

### 1. Build the vulnerable image

`docker build -t trivy-lab:v1 TrivyImage/.` builds from `node:18-bullseye`.

![Build trivy-lab v1](image-2.png)

### 2. Scan the vulnerable image

`trivy image --severity HIGH,CRITICAL trivy-lab:v1` detects Debian 11.11 and reports that the OS is no longer supported.

![Trivy scan of v1](image.png)

### 3. CVE details (part 1)

Trivy table output with the CVE list (tar traversal and related issues).

![CVE list part 1](image-1.png)

### 4. CVE details (part 2)

More CVE rows from the same scan, including CRITICAL `CVE-2022-37454` (python3.9) and HIGH `CVE-2025-13836`.

![CVE list part 2](image-6.png)

### 5. HTML report (custom template)

`trivy image --format template --template "@html.tpl" --output report.html --severity HIGH,CRITICAL trivy-lab:v1`, then `open report.html`.

![HTML report with custom template](image-3.png)

![img.png](img.png)
### 6. HTML report (built-in template)

Same scan using the built-in template `@contrib/html.tpl` written to `report.html`.

![HTML report with built-in template](image-4.png)

### 7. Rebuild on the updated base image

`trivy-lab:v4` built on `node:24-trixie-slim` (Debian 13.7) and scanned.

![Build and scan trivy-lab v4](image-5.png)

### 8. Critical issues gone

`grep critical` shows a critical CVE on `trivy-lab:v1` and none on `trivy-lab:v4`.

![Critical comparison v1 vs v4](image-7.png)
