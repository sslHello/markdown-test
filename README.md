# markdown-test
Some tests with markdown
---
---
<table style="align:center; border-collapse: collapse; text-align:center; margin: 0px 5px 0px 5px; border: 3px solid #444444; background-color: #FFFFFF; padding=2;">
<tr style="background-color: #FFFFFF; border: 3px solid #444444; height: 2em; font-size: 100%; color: #FFFFFF;  text-shadow: 2px 2px 8px #000000; ">
         <th width="16.5%" style="border: 3px solid #444444;"> Threat Agents</th>
         <th width="16.5%" style="border: 3px solid #444444;"> Attack Vectors</th>
         <th width="33%" colspan="2" style="border: 3px solid #444444;"> Security Weakness</th>
         <th width="16.5%" style="border: 3px solid #444444;">Technical Impacts</th>
         <th width="16.5%" style="border: 3px solid #444444;">Business Impacts</th>
</tr>
<tr>
    <td font-size="100%"; font-weight="bold"; background-color="#D9D9D9"; color="#000000"; border="3px solid #444444"><b>Application<br/>Specific</b></td>
    <td><b>Exploitability<br/> EASY</b></td>
    <td><b>Prevalence<br/> COMMON</b></td>
    <td><b>Detectability<br/> COMMON</b></td>
    <td><b>Impact<br/> COMMON</b></td>
    <td><b>Application /<br/> Business Specific </b></td>
 </tr>
 <!---- Content ---->
 <tr valign="top";> 
     <!--- Threat Agents --->
     <td>
     Consider anyone who can send untrusted data to the system, including external users, internal users, and administrators.
     </td>
     <!--- Attack Vectors --->
     <td>
     Attackers send simple text-based attacks that exploit the syntax of the targeted interpreter. Almost any source of data can be an injection vector, including internal sources. 
     </td>
     <!---  Security Weakness --->
     <td colspan="2";>
     <!--- internal OWASP-Link---> <u><a href="https://www.owasp.org/index.php/Injection_Flaws">Injection flaws</a></u> occur when an application sends untrusted data to an interpreter. Injection flaws are very prevalent, particularly in legacy code. They are often found in SQL, LDAP, XPath, or NoSQL queries; OS commands; XML parsers, SMTP Headers, expression languages, etc. Injection flaws are easy to discover when examining code, but frequently hard to discover via testing. Scanners and fuzzers can help attackers find injection flaws. 
     </td>
     <!--- Technical Impacts --->
     <td>
     Injection can result in data loss or corruption, lack of accountability, or denial of access. Injection can sometimes lead to complete host takeover. 
     </td>
     <!--- Business Impacts --->
     <td>
     Consider the business value of the affected data and the platform running the interpreter. All data could be stolen, modified, or deleted. Could your reputation be harmed?
     </td>
 </tr>
 </table>
     
<!--- {{Top_10:SubsectionTableBeginTemplate|type=main}} {{Top_10_2010:SubsectionAdvancedTemplate|type={{Top_10_2010:StyleTemplate}}|subsection=vulnerableTo|position=firstLeft|risk=1|year=2017|language=en}} ---> 
<table>
<tr valign="top";> 
    <td width="50%"><b>Am I Vulnerable To 'Injection'?</b></td>
    <td width="50%"><b>How Do I Prevent 'Injection'?</b></td>
</tr>
<tr valign="top";> 
    <td  width="50%">The best way to find out if an application is vulnerable to injection is to verify that <u>all</u> use of interpreters clearly separates untrusted data from the command or query. In many cases, it is recommended to avoid the interpreter, or disable it (e.g., XXE), if possible. For SQL calls, use bind variables in all prepared statements and stored procedures, or avoid dynamic queries.
Checking the code is a fast and accurate way to see if the application uses interpreters safely. Code analysis tools can help a security analyst find use of interpreters and trace data flow through the application. Penetration testers can validate these issues by crafting exploits that confirm the vulnerability.
Automated dynamic scanning which exercises the application may provide insight into whether some exploitable injection flaws exist. Scanners cannot always reach interpreters and have difficulty detecting whether an attack was successful. Poor error handling makes injection flaws easier to discover.</td>
 <!--- {{Top_10_2010:SubsectionAdvancedTemplate|type={{Top_10_2010:StyleTemplate}}|subsection=howPrevent|position=right|risk=1|year=2017|language=en}}  --->
   <td width="50%">Preventing injection requires keeping untrusted data separate from commands and queries.
# The preferred option is to use a safe API which avoids the use of the interpreter entirely or provides a parameterized interface.  Be careful with APIs, such as stored procedures, that are parameterized, but can still introduce injection under the hood.
# If a parameterized API is not available, you should carefully escape special characters using the specific escape syntax for that interpreter. <u>[[OWASP_Java_Encoder_Project|OWASP’s Java Encoder]]</u> and similar libraries provide such escaping routines.
# Positive or “white list” input validation is also recommended, but is <u>not</u> a complete defense as many situations require special characters be allowed. If special characters are required, only approaches (1) and (2) above will make their use safe. <u>[[ESAPI|OWASP’s ESAPI]]</u> has an extensible library of <u>[https://static.javadoc.io/org.owasp.esapi/esapi/2.1.0.1/org/owasp/esapi/Validator.html white list input validation routines]</u>.</td>
</tr>
<tr valign="top";> 
    <td width="50%"><b>Example Attack Scenarios</b></td>
    <td width="50%"><b>References</b></td>
</tr>
<tr valign="top";> 
    <td width="50%">
 <!--- {{Top_10_2010:SubsectionAdvancedTemplate|type={{Top_10_2010:StyleTemplate}}|subsection=example|position=left|risk=1|year=2017|language=en}} --->
    <u><b>Scenario #1:</b></u> An application uses untrusted data in the construction of the following '''vulnerable''' SQL call:

 <!--- {{Top_10_2010:ExampleBeginTemplate|year=2017}} --->
      <b><span style="color:red;">
      String query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";
      </span></b> 
 <!--- {{Top_10_2010:ExampleEndTemplate}} --->

<u><b>Scenario #2:</b></u> Similarly, an application’s blind trust in frameworks may result in queries that are still vulnerable, (e.g., Hibernate Query Language (HQL)):
 <!--- {{Top_10_2010:ExampleBeginTemplate|year=2017}} --->
    <b><span style="color:red;">
    Query HQLQuery = session.createQuery("FROM accounts
    WHERE custID='" + request.getParameter("id") + "'");'
    </span></b>
 <!--- {{Top_10_2010:ExampleEndTemplate}} --->
In both cases, the attacker modifies the ‘id’ parameter value in her browser to send: <span style="color:red;">' or '1'='1</span>. For example: 
 <!--- {{Top_10_2010:ExampleBeginTemplate|year=2017}} --->
 <b><span style="color:red;"><nowiki>
http://example.com/app/accountView?id=' or '1'='1 
</nowiki></span></b>
 <!--- {{Top_10_2010:ExampleEndTemplate}} --->
This changes the meaning of both queries to return all the records from the accounts table. More dangerous attacks could modify data or even invoke stored procedures.
    </td>
    <td width="50%">

<b>OWASP</b>
* <u>[[SQL_Injection_Prevention_Cheat_Sheet | OWASP SQL Injection Prevention Cheat Sheet]]</u>
* <u>[[Query_Parameterization_Cheat_Sheet | OWASP Query Parameterization Cheat Sheet]]</u>
* <u>[[Command_Injection | OWASP Command Injection Article]]</u>
* <u>[[XML_External_Entity_(XXE)_Prevention_Cheat_Sheet| OWASP XXE Prevention Cheat Sheet]]</u>
* <u>[[Testing_for_SQL_Injection_(OTG-INPVAL-005)|OWASP Testing Guide: Chapter on SQL Injection Testing]]</u>

 <!--- {{Top_10_2010:SubSubsectionExternalReferencesTemplate}} --->
<b>External</b>
* <u>[http://cwe.mitre.org/data/definitions/77.html CWE Entry 77 on Command Injection]</u>
* <u>[http://cwe.mitre.org/data/definitions/89.html CWE Entry 89 on SQL Injection]</u>
* <u>[http://cwe.mitre.org/data/definitions/564.html CWE Entry 564 on Hibernate Injection]</u>
* <u>[http://cwe.mitre.org/data/definitions/611.html CWE Entry 611 on Improper Restriction of XXE]</u>
* <u>[http://cwe.mitre.org/data/definitions/917.html|CWE Entry 917 on Expression Language Injection]</u>
    </td>
</tr>
</table>
 
