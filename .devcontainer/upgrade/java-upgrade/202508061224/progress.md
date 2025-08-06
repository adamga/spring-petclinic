# Upgrade Progress

  ### ✅ Generate Upgrade Plan [View Log](logs\1.generatePlan.log)

  ### ✅ Confirm Upgrade Plan [View Log](logs\2.confirmPlan.log)
  
  <details>
      <summary>[ click to toggle details ]</summary>
  
  - ###
    ### ✅ Precheck - Build project [View Log](logs\2.1.precheck-buildProject.log)
  
    ### ✅ Precheck - Validate CVEs [View Log](logs\2.2.precheck-validateCves.log)
    
    <details>
        <summary>[ click to toggle details ]</summary>
    
    #### CVE issues
    - Dependency `com.mysql:mysql-connector-j` has **1** known CVEs:
      - [CVE-2023-22102](https://github.com/advisories/GHSA-m6vm-37g8-gqvh): MySQL Connectors takeover vulnerability
        - **Severity**: **HIGH**
        - **Details**: Vulnerability in the MySQL Connectors product of Oracle MySQL (component: Connector/J). Supported versions that are affected are 8.1.0 and prior. Difficult to exploit vulnerability allows unauthenticated attacker with network access via multiple protocols to compromise MySQL Connectors. Successful attacks require human interaction from a person other than the attacker and while the vulnerability is in MySQL Connectors, attacks may significantly impact additional products (scope change). Successful attacks of this vulnerability can result in takeover of MySQL Connectors.
    - Dependency `org.postgresql:postgresql` has **1** known CVEs:
      - [CVE-2024-1597](https://github.com/advisories/GHSA-24rp-q3w6-vc56): org.postgresql:postgresql vulnerable to SQL Injection via line comment generation
        - **Severity**: **CRITICAL**
        - **Details**: # Impact
          SQL injection is possible when using the non-default connection property `preferQueryMode=simple` in combination with application code that has a vulnerable SQL that negates a parameter value.
          
          There is no vulnerability in the driver when using the default query mode. Users that do not override the query mode are not impacted.
          
          # Exploitation
          
          To exploit this behavior the following conditions must be met:
          
          1. A placeholder for a numeric value must be immediately preceded by a minus (i.e. `-`)
          1. There must be a second placeholder for a string value after the first placeholder on the same line. 
          1. Both parameters must be user controlled.
          
          The prior behavior of the driver when operating in simple query mode would inline the negative value of the first parameter and cause the resulting line to be treated as a `--` SQL comment. That would extend to the beginning of the next parameter and cause the quoting of that parameter to be consumed by the comment line. If that string parameter includes a newline, the resulting text would appear unescaped in the resulting SQL.
          
          When operating in the default extended query mode this would not be an issue as the parameter values are sent separately to the server. Only in simple query mode the parameter values are inlined into the executed SQL causing this issue.
          
          # Example
          
          ```java
          PreparedStatement stmt = conn.prepareStatement("SELECT -?, ?");
          stmt.setInt(1, -1);
          stmt.setString(2, "\nWHERE false --");
          ResultSet rs = stmt.executeQuery();
          ```
          
          The resulting SQL when operating in simple query mode would be:
          
          ```sql
          SELECT --1,'
          WHERE false --'
          ```
          
          The contents of the second parameter get injected into the command. Note how both the number of result columns and the WHERE clause of the command have changed. A more elaborate example could execute arbitrary other SQL commands.
          
          # Patch
          Problem will be patched upgrade to 42.7.2, 42.6.1, 42.5.5, 42.4.4, 42.3.9, 42.2.28, 42.2.28.jre7
          
          The patch fixes the inlining of parameters by forcing them all to be serialized as wrapped literals. The SQL in the prior example would be transformed into:
          
          ```sql
          SELECT -('-1'::int4), ('
          WHERE false --')
          ```
          
          # Workarounds
          Do not use the connection property`preferQueryMode=simple`. (*NOTE: If you do not explicitly specify a query mode then you are using the default of `extended` and are not impacted by this issue.*)
    
    
    
    </details>
  
    ### ✅ Precheck - Run tests [View Log](logs\2.3.precheck-runTests.log)
    
    <details>
        <summary>[ click to toggle details ]</summary>
    
    #### Test result
    | Total | Passed | Failed | Skipped | Errors |
    |-------|--------|--------|---------|--------|
    | 45 | 43 | 0 | 2 | 0 |
    
    
    
    </details>
  
  </details>

  ### ✅ Upgrade project to Java 21
  
  <details>
      <summary>[ click to toggle details ]</summary>
  
  - ###
    ### ✅ Upgrade using OpenRewrite [View Log](logs\3.1.upgradeProjectUsingOpenRewrite.log)
    1 file changed, 3 insertions(+), 3 deletions(-)
    <details>
        <summary>[ click to toggle details ]</summary>
    
    #### Recipes
    - [org.openrewrite.java.migrate.UpgradeToJava21](https://docs.openrewrite.org/recipes/java/migrate/UpgradeToJava21)
    
    
    
    </details>
  
    ### ✅ Build Project [View Log](logs\3.2.buildProject.log)
  
  </details>

  ### ✅ Validate CVEs [View Log](logs\4.validateCves.log)
  
  <details>
      <summary>[ click to toggle details ]</summary>
  
  #### Checked Dependencies
    - org.springframework.boot:spring-boot-starter-actuator:3.2.1
    - org.springframework.boot:spring-boot-starter:3.2.1
    - ch.qos.logback:logback-classic:1.4.14
    - org.springframework:spring-context:6.1.2
    - org.springframework:spring-core:6.1.2
    - org.hibernate.orm:hibernate-core:6.4.1.Final
    - org.apache.tomcat.embed:tomcat-embed-core:10.1.17
    - com.fasterxml.jackson.datatype:jackson-datatype-jdk8:2.15.3
    - com.zaxxer:HikariCP:5.0.1
  
  #### CVE issues
  - Dependency `org.springframework:spring-context` has **2** known CVEs:
    - [CVE-2024-38820](https://github.com/advisories/GHSA-4gc7-5j7h-4qph): Spring Framework DataBinder Case Sensitive Match Exception
      - **Severity**: **MEDIUM**
      - **Details**: The fix for CVE-2022-22968 made disallowedFields patterns in DataBinder case insensitive. However, String.toLowerCase() has some Locale dependent exceptions that could potentially result in fields not protected as expected.
    - [CVE-2025-22233](https://github.com/advisories/GHSA-4wp7-92pw-q264): Spring Framework DataBinder Case Sensitive Match Exception
      - **Severity**: **LOW**
      - **Details**: CVE-2024-38820 ensured Locale-independent, lowercase conversion for both the configured disallowedFields patterns and for request parameter names. However, there are still cases where it is possible to bypass the disallowedFields checks.
        
        Affected Spring Products and Versions
        
        Spring Framework:
          *  6.2.0 - 6.2.6
        
          *  6.1.0 - 6.1.19
        
          *  6.0.0 - 6.0.27
        
          *  5.3.0 - 5.3.42
          *  Older, unsupported versions are also affected
        
        
        
        Mitigation
        
        Users of affected versions should upgrade to the corresponding fixed version.
        
        | Affected version(s) | Fix Version | Availability |
        | - | - | - |
        | 6.2.x |  6.2.7 | OSS |
        | 6.1.x |  6.1.20 | OSS |
        | 6.0.x |  6.0.28 |  Commercial https://enterprise.spring.io/ |
        | 5.3.x |  5.3.43 | Commercial https://enterprise.spring.io/  |
        
        No further mitigation steps are necessary.
        
        
        Generally, we recommend using a dedicated model object with properties only for data binding, or using constructor binding since constructor arguments explicitly declare what to bind together with turning off setter binding through the declarativeBinding flag. See the Model Design section in the reference documentation.
        
        For setting binding, prefer the use of allowedFields (an explicit list) over disallowedFields.
        
        Credit
        
        This issue was responsibly reported by the TERASOLUNA Framework Development Team from NTT DATA Group Corporation.
  - Dependency `org.springframework:spring-core` has **1** known CVEs:
    - [CVE-2024-22233](https://github.com/advisories/GHSA-r4q3-7g4q-x89m): Spring Framework server Web DoS Vulnerability
      - **Severity**: **HIGH**
      - **Details**: In Spring Framework versions 6.0.15 and 6.1.2, it is possible for a user to provide specially crafted HTTP requests that may cause a denial-of-service (DoS) condition.
        
        Specifically, an application is vulnerable when all of the following are true:
        
          *  the application uses Spring MVC
          *  Spring Security 6.1.6+ or 6.2.1+ is on the classpath
        
        
        Typically, Spring Boot applications need the org.springframework.boot:spring-boot-starter-web and org.springframework.boot:spring-boot-starter-security dependencies to meet all conditions.
  - Dependency `org.apache.tomcat.embed:tomcat-embed-core` has **10** known CVEs:
    - [CVE-2024-24549](https://github.com/advisories/GHSA-7w75-32cg-r6g2): Apache Tomcat Denial of Service due to improper input validation vulnerability for HTTP/2 requests
      - **Severity**: **MEDIUM**
      - **Details**: Denial of Service due to improper input validation vulnerability for HTTP/2 requests in Apache Tomcat. When processing an HTTP/2 request, if the request exceeded any of the configured limits for headers, the associated HTTP/2 stream was not reset until after all of the headers had been processed.This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.0-M16, from 10.1.0-M1 through 10.1.18, from 9.0.0-M1 through 9.0.85, from 8.5.0 through 8.5.98.
        
        Users are recommended to upgrade to version 11.0.0-M17, 10.1.19, 9.0.86 or 8.5.99 which fix the issue.
    - [CVE-2024-34750](https://github.com/advisories/GHSA-wm9w-rjj3-j356): Apache Tomcat - Denial of Service
      - **Severity**: **HIGH**
      - **Details**: Improper Handling of Exceptional Conditions, Uncontrolled Resource Consumption vulnerability in Apache Tomcat. When processing an HTTP/2 stream, Tomcat did not handle some cases of excessive HTTP headers correctly. This led to a miscounting of active HTTP/2 streams which in turn led to the use of an incorrect infinite timeout which allowed connections to remain open which should have been closed.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.0-M20, from 10.1.0-M1 through 10.1.24, from 9.0.0-M1 through 9.0.89.
        
        Users are recommended to upgrade to version 11.0.0-M21, 10.1.25 or 9.0.90, which fixes the issue.
    - [CVE-2024-50379](https://github.com/advisories/GHSA-5j33-cvvr-w245): Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability
      - **Severity**: **HIGH**
      - **Details**: Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability during JSP compilation in Apache Tomcat permits an RCE on case insensitive file systems when the default servlet is enabled for write (non-default configuration).
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.1, from 10.1.0-M1 through 10.1.33, from 9.0.0.M1 through 9.0.97.
        
        Users are recommended to upgrade to version 11.0.2, 10.1.34 or 9.0.98, which fixes the issue.
    - [CVE-2024-56337](https://github.com/advisories/GHSA-27hp-xhwr-wr2m): Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability
      - **Severity**: **HIGH**
      - **Details**: Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability in Apache Tomcat.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.1, from 10.1.0-M1 through 10.1.33, from 9.0.0.M1 through 9.0.97.
        
        The mitigation for CVE-2024-50379 was incomplete.
        
        Users running Tomcat on a case insensitive file system with the default servlet write enabled (readonly initialisation 
        parameter set to the non-default value of false) may need additional configuration to fully mitigate CVE-2024-50379 depending on which version of Java they are using with Tomcat:
        - running on Java 8 or Java 11: the system property sun.io.useCanonCaches must be explicitly set to false (it defaults to true)
        - running on Java 17: the system property sun.io.useCanonCaches, if set, must be set to false (it defaults to false)
        - running on Java 21 onwards: no further configuration is required (the system property and the problematic cache have been removed)
        
        Tomcat 11.0.3, 10.1.35 and 9.0.99 onwards will include checks that sun.io.useCanonCaches is set appropriately before allowing the default servlet to be write enabled on a case insensitive file system. Tomcat will also set sun.io.useCanonCaches to false by default where it can.
    - [CVE-2025-24813](https://github.com/advisories/GHSA-83qj-6fr2-vhqg): Apache Tomcat: Potential RCE and/or information disclosure and/or information corruption with partial PUT
      - **Severity**: **CRITICAL**
      - **Details**: Path Equivalence: 'file.Name' (Internal Dot) leading to Remote Code Execution and/or Information disclosure and/or malicious content added to uploaded files via write enabled Default Servlet in Apache Tomcat.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.2, from 10.1.0-M1 through 10.1.34, from 9.0.0.M1 through 9.0.98.
        
        If all of the following were true, a malicious user was able to view security sensitive files and/or inject content into those files:
        - writes enabled for the default servlet (disabled by default)
        - support for partial PUT (enabled by default)
        - a target URL for security sensitive uploads that was a sub-directory of a target URL for public uploads
        - attacker knowledge of the names of security sensitive files being uploaded
        - the security sensitive files also being uploaded via partial PUT
        
        If all of the following were true, a malicious user was able to perform remote code execution:
        - writes enabled for the default servlet (disabled by default)
        - support for partial PUT (enabled by default)
        - application was using Tomcat's file based session persistence with the default storage location
        - application included a library that may be leveraged in a deserialization attack
        
        Users are recommended to upgrade to version 11.0.3, 10.1.35 or 9.0.99, which fixes the issue.
    - [CVE-2025-31650](https://github.com/advisories/GHSA-3p2h-wqq4-wf4h): Apache Tomcat Denial of Service via invalid HTTP priority header
      - **Severity**: **MEDIUM**
      - **Details**: Improper Input Validation vulnerability in Apache Tomcat. Incorrect error handling for some invalid HTTP priority headers resulted in incomplete clean-up of the failed request which created a memory leak. A large number of such requests could trigger an OutOfMemoryException resulting in a denial of service.
        
        This issue affects Apache Tomcat: from 9.0.76 through 9.0.102, from 10.1.10 through 10.1.39, from 11.0.0-M2 through 11.0.5.
        
        Users are recommended to upgrade to version 9.0.104, 10.1.40 or 11.0.6 which fix the issue.
    - [CVE-2025-31651](https://github.com/advisories/GHSA-ff77-26x5-69cr): Apache Tomcat Rewrite rule bypass
      - **Severity**: **LOW**
      - **Details**: Improper Neutralization of Escape, Meta, or Control Sequences vulnerability in Apache Tomcat. For a subset of unlikely rewrite rule configurations, it was possible for a specially crafted request to bypass some rewrite rules. If those rewrite rules effectively enforced security constraints, those constraints could be bypassed.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.5, from 10.1.0-M1 through 10.1.39, from 9.0.0.M1 through 9.0.102.
        
        Users are recommended to upgrade to version 9.0.104, 10.1.40 or 11.0.6, which fix the issue.
    - [CVE-2025-46701](https://github.com/advisories/GHSA-h2fw-rfh5-95r3): Apache Tomcat - CGI security constraint bypass
      - **Severity**: **LOW**
      - **Details**: Improper Handling of Case Sensitivity vulnerability in Apache Tomcat's GCI servlet allows security constraint bypass of security constraints that apply to the pathInfo component of a URI mapped to the CGI servlet.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.6, from 10.1.0-M1 through 10.1.40, from 9.0.0.M1 through 9.0.104.
        
        Users are recommended to upgrade to version 11.0.7, 10.1.41 or 9.0.105, which fixes the issue.
    - [CVE-2025-48988](https://github.com/advisories/GHSA-h3gc-qfqq-6h8f): Apache Tomcat - DoS in multipart upload
      - **Severity**: **HIGH**
      - **Details**: Allocation of Resources Without Limits or Throttling vulnerability in Apache Tomcat.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.7, from 10.1.0-M1 through 10.1.41, from 9.0.0.M1 through 9.0.105.
        
        Users are recommended to upgrade to version 11.0.8, 10.1.42 or 9.0.106, which fix the issue.
    - [CVE-2025-49125](https://github.com/advisories/GHSA-wc4r-xq3c-5cf3): Apache Tomcat - Security constraint bypass for pre/post-resources
      - **Severity**: **MEDIUM**
      - **Details**: Authentication Bypass Using an Alternate Path or Channel vulnerability in Apache Tomcat.  When using PreResources or PostResources mounted other than at the root of the web application, it was possible to access those resources via an unexpected path. That path was likely not to be protected by the same security constraints as the expected path, allowing those security constraints to be bypassed.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.7, from 10.1.0-M1 through 10.1.41, from 9.0.0.M1 through 9.0.105.
        
        Users are recommended to upgrade to version 11.0.8, 10.1.42 or 9.0.106, which fix the issue.
  
  
  
  </details>

  ### ✅ Fix CVE Issues [View Log](logs\5.fixCveIssues.log)
  1 file changed, 1 insertion(+), 1 deletion(-)
  <details>
      <summary>[ click to toggle details ]</summary>
  
  #### Code changes
  - Upgrade Spring Boot version from 3.2.1 to 3.2.12
    - Updated spring-boot-starter-parent version to 3.2.12 to include latest security fixes and dependency updates
  
  
  
  </details>

  ### ✅ Build Project [View Log](logs\6.buildProject.log)

  ### ✅ Validate CVEs [View Log](logs\7.validateCves.log)
  
  <details>
      <summary>[ click to toggle details ]</summary>
  
  #### Checked Dependencies
    - org.springframework.boot:spring-boot-starter-actuator:3.2.12
    - org.springframework.boot:spring-boot-starter:3.2.12
    - ch.qos.logback:logback-classic:1.4.14
    - org.springframework:spring-context:6.1.15
    - org.springframework:spring-core:6.1.15
    - org.hibernate.orm:hibernate-core:6.4.10.Final
    - org.apache.tomcat.embed:tomcat-embed-core:10.1.33
    - com.fasterxml.jackson.datatype:jackson-datatype-jdk8:2.15.4
    - com.zaxxer:HikariCP:5.0.1
  
  #### CVE issues
  - Dependency `org.springframework:spring-context` has **1** known CVEs:
    - [CVE-2025-22233](https://github.com/advisories/GHSA-4wp7-92pw-q264): Spring Framework DataBinder Case Sensitive Match Exception
      - **Severity**: **LOW**
      - **Details**: CVE-2024-38820 ensured Locale-independent, lowercase conversion for both the configured disallowedFields patterns and for request parameter names. However, there are still cases where it is possible to bypass the disallowedFields checks.
        
        Affected Spring Products and Versions
        
        Spring Framework:
          *  6.2.0 - 6.2.6
        
          *  6.1.0 - 6.1.19
        
          *  6.0.0 - 6.0.27
        
          *  5.3.0 - 5.3.42
          *  Older, unsupported versions are also affected
        
        
        
        Mitigation
        
        Users of affected versions should upgrade to the corresponding fixed version.
        
        | Affected version(s) | Fix Version | Availability |
        | - | - | - |
        | 6.2.x |  6.2.7 | OSS |
        | 6.1.x |  6.1.20 | OSS |
        | 6.0.x |  6.0.28 |  Commercial https://enterprise.spring.io/ |
        | 5.3.x |  5.3.43 | Commercial https://enterprise.spring.io/  |
        
        No further mitigation steps are necessary.
        
        
        Generally, we recommend using a dedicated model object with properties only for data binding, or using constructor binding since constructor arguments explicitly declare what to bind together with turning off setter binding through the declarativeBinding flag. See the Model Design section in the reference documentation.
        
        For setting binding, prefer the use of allowedFields (an explicit list) over disallowedFields.
        
        Credit
        
        This issue was responsibly reported by the TERASOLUNA Framework Development Team from NTT DATA Group Corporation.
  - Dependency `org.apache.tomcat.embed:tomcat-embed-core` has **8** known CVEs:
    - [CVE-2024-50379](https://github.com/advisories/GHSA-5j33-cvvr-w245): Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability
      - **Severity**: **HIGH**
      - **Details**: Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability during JSP compilation in Apache Tomcat permits an RCE on case insensitive file systems when the default servlet is enabled for write (non-default configuration).
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.1, from 10.1.0-M1 through 10.1.33, from 9.0.0.M1 through 9.0.97.
        
        Users are recommended to upgrade to version 11.0.2, 10.1.34 or 9.0.98, which fixes the issue.
    - [CVE-2024-56337](https://github.com/advisories/GHSA-27hp-xhwr-wr2m): Apache Tomcat Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability
      - **Severity**: **HIGH**
      - **Details**: Time-of-check Time-of-use (TOCTOU) Race Condition vulnerability in Apache Tomcat.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.1, from 10.1.0-M1 through 10.1.33, from 9.0.0.M1 through 9.0.97.
        
        The mitigation for CVE-2024-50379 was incomplete.
        
        Users running Tomcat on a case insensitive file system with the default servlet write enabled (readonly initialisation 
        parameter set to the non-default value of false) may need additional configuration to fully mitigate CVE-2024-50379 depending on which version of Java they are using with Tomcat:
        - running on Java 8 or Java 11: the system property sun.io.useCanonCaches must be explicitly set to false (it defaults to true)
        - running on Java 17: the system property sun.io.useCanonCaches, if set, must be set to false (it defaults to false)
        - running on Java 21 onwards: no further configuration is required (the system property and the problematic cache have been removed)
        
        Tomcat 11.0.3, 10.1.35 and 9.0.99 onwards will include checks that sun.io.useCanonCaches is set appropriately before allowing the default servlet to be write enabled on a case insensitive file system. Tomcat will also set sun.io.useCanonCaches to false by default where it can.
    - [CVE-2025-24813](https://github.com/advisories/GHSA-83qj-6fr2-vhqg): Apache Tomcat: Potential RCE and/or information disclosure and/or information corruption with partial PUT
      - **Severity**: **CRITICAL**
      - **Details**: Path Equivalence: 'file.Name' (Internal Dot) leading to Remote Code Execution and/or Information disclosure and/or malicious content added to uploaded files via write enabled Default Servlet in Apache Tomcat.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.2, from 10.1.0-M1 through 10.1.34, from 9.0.0.M1 through 9.0.98.
        
        If all of the following were true, a malicious user was able to view security sensitive files and/or inject content into those files:
        - writes enabled for the default servlet (disabled by default)
        - support for partial PUT (enabled by default)
        - a target URL for security sensitive uploads that was a sub-directory of a target URL for public uploads
        - attacker knowledge of the names of security sensitive files being uploaded
        - the security sensitive files also being uploaded via partial PUT
        
        If all of the following were true, a malicious user was able to perform remote code execution:
        - writes enabled for the default servlet (disabled by default)
        - support for partial PUT (enabled by default)
        - application was using Tomcat's file based session persistence with the default storage location
        - application included a library that may be leveraged in a deserialization attack
        
        Users are recommended to upgrade to version 11.0.3, 10.1.35 or 9.0.99, which fixes the issue.
    - [CVE-2025-31650](https://github.com/advisories/GHSA-3p2h-wqq4-wf4h): Apache Tomcat Denial of Service via invalid HTTP priority header
      - **Severity**: **MEDIUM**
      - **Details**: Improper Input Validation vulnerability in Apache Tomcat. Incorrect error handling for some invalid HTTP priority headers resulted in incomplete clean-up of the failed request which created a memory leak. A large number of such requests could trigger an OutOfMemoryException resulting in a denial of service.
        
        This issue affects Apache Tomcat: from 9.0.76 through 9.0.102, from 10.1.10 through 10.1.39, from 11.0.0-M2 through 11.0.5.
        
        Users are recommended to upgrade to version 9.0.104, 10.1.40 or 11.0.6 which fix the issue.
    - [CVE-2025-31651](https://github.com/advisories/GHSA-ff77-26x5-69cr): Apache Tomcat Rewrite rule bypass
      - **Severity**: **LOW**
      - **Details**: Improper Neutralization of Escape, Meta, or Control Sequences vulnerability in Apache Tomcat. For a subset of unlikely rewrite rule configurations, it was possible for a specially crafted request to bypass some rewrite rules. If those rewrite rules effectively enforced security constraints, those constraints could be bypassed.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.5, from 10.1.0-M1 through 10.1.39, from 9.0.0.M1 through 9.0.102.
        
        Users are recommended to upgrade to version 9.0.104, 10.1.40 or 11.0.6, which fix the issue.
    - [CVE-2025-46701](https://github.com/advisories/GHSA-h2fw-rfh5-95r3): Apache Tomcat - CGI security constraint bypass
      - **Severity**: **LOW**
      - **Details**: Improper Handling of Case Sensitivity vulnerability in Apache Tomcat's GCI servlet allows security constraint bypass of security constraints that apply to the pathInfo component of a URI mapped to the CGI servlet.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.6, from 10.1.0-M1 through 10.1.40, from 9.0.0.M1 through 9.0.104.
        
        Users are recommended to upgrade to version 11.0.7, 10.1.41 or 9.0.105, which fixes the issue.
    - [CVE-2025-48988](https://github.com/advisories/GHSA-h3gc-qfqq-6h8f): Apache Tomcat - DoS in multipart upload
      - **Severity**: **HIGH**
      - **Details**: Allocation of Resources Without Limits or Throttling vulnerability in Apache Tomcat.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.7, from 10.1.0-M1 through 10.1.41, from 9.0.0.M1 through 9.0.105.
        
        Users are recommended to upgrade to version 11.0.8, 10.1.42 or 9.0.106, which fix the issue.
    - [CVE-2025-49125](https://github.com/advisories/GHSA-wc4r-xq3c-5cf3): Apache Tomcat - Security constraint bypass for pre/post-resources
      - **Severity**: **MEDIUM**
      - **Details**: Authentication Bypass Using an Alternate Path or Channel vulnerability in Apache Tomcat.  When using PreResources or PostResources mounted other than at the root of the web application, it was possible to access those resources via an unexpected path. That path was likely not to be protected by the same security constraints as the expected path, allowing those security constraints to be bypassed.
        
        This issue affects Apache Tomcat: from 11.0.0-M1 through 11.0.7, from 10.1.0-M1 through 10.1.41, from 9.0.0.M1 through 9.0.105.
        
        Users are recommended to upgrade to version 11.0.8, 10.1.42 or 9.0.106, which fix the issue.
  
  
  
  </details>

  ### ✅ Fix CVE Issues [View Log](logs\8.fixCveIssues.log)
  1 file changed, 3 insertions(+), 0 deletions(-)
  <details>
      <summary>[ click to toggle details ]</summary>
  
  #### Code changes
  - Override Tomcat version to 10.1.42 for security fixes
    - Added tomcat.version property to override the default Tomcat version from Spring Boot
  
  
  
  </details>

  ### ✅ Build Project [View Log](logs\9.buildProject.log)

  ### ✅ Validate CVEs [View Log](logs\10.validateCves.log)
  
  <details>
      <summary>[ click to toggle details ]</summary>
  
  #### Checked Dependencies
    - org.springframework.boot:spring-boot-starter-actuator:3.2.12
    - org.springframework.boot:spring-boot-starter:3.2.12
    - ch.qos.logback:logback-classic:1.4.14
    - org.springframework:spring-context:6.1.15
    - org.springframework:spring-core:6.1.15
    - org.hibernate.orm:hibernate-core:6.4.10.Final
    - org.apache.tomcat.embed:tomcat-embed-core:10.1.42
    - com.fasterxml.jackson.datatype:jackson-datatype-jdk8:2.15.4
    - com.zaxxer:HikariCP:5.0.1
  
  #### CVE issues
  - Dependency `org.springframework:spring-context` has **1** known CVEs:
    - [CVE-2025-22233](https://github.com/advisories/GHSA-4wp7-92pw-q264): Spring Framework DataBinder Case Sensitive Match Exception
      - **Severity**: **LOW**
      - **Details**: CVE-2024-38820 ensured Locale-independent, lowercase conversion for both the configured disallowedFields patterns and for request parameter names. However, there are still cases where it is possible to bypass the disallowedFields checks.
        
        Affected Spring Products and Versions
        
        Spring Framework:
          *  6.2.0 - 6.2.6
        
          *  6.1.0 - 6.1.19
        
          *  6.0.0 - 6.0.27
        
          *  5.3.0 - 5.3.42
          *  Older, unsupported versions are also affected
        
        
        
        Mitigation
        
        Users of affected versions should upgrade to the corresponding fixed version.
        
        | Affected version(s) | Fix Version | Availability |
        | - | - | - |
        | 6.2.x |  6.2.7 | OSS |
        | 6.1.x |  6.1.20 | OSS |
        | 6.0.x |  6.0.28 |  Commercial https://enterprise.spring.io/ |
        | 5.3.x |  5.3.43 | Commercial https://enterprise.spring.io/  |
        
        No further mitigation steps are necessary.
        
        
        Generally, we recommend using a dedicated model object with properties only for data binding, or using constructor binding since constructor arguments explicitly declare what to bind together with turning off setter binding through the declarativeBinding flag. See the Model Design section in the reference documentation.
        
        For setting binding, prefer the use of allowedFields (an explicit list) over disallowedFields.
        
        Credit
        
        This issue was responsibly reported by the TERASOLUNA Framework Development Team from NTT DATA Group Corporation.
  
  
  
  </details>

  ### ✅ Validate Code Behavior Changes [View Log](logs\11.validateBehaviorChanges.log)

  ### ✅ Run Tests [View Log](logs\12.runTests.log)
  
  <details>
      <summary>[ click to toggle details ]</summary>
  
  #### Test result
  | Total | Passed | Failed | Skipped | Errors |
  |-------|--------|--------|---------|--------|
  | 45 | 43 | 0 | 2 | 0 |
  
  
  
  </details>

  ### ✅ Summarize Upgrade [View Log](logs\13.summarizeUpgrade.log)