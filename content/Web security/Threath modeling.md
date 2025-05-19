Threth modeling may address 4 questions
#### What are we building?
To answer this wuestiong we land what's called **system modeling**, which is a min map of our system functionalities. A common way to represent the backend functionallity is by representing an schema as **data flow diagrams (DFDs)**, to create mind map of the functionallity. Example of Softwares for doing this are:
- [OWASP's Threat Dragon](https://github.com/OWASP/threat-dragon) 
- [Microsoft's Threat Modeling Tool](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool)
- [draw.io](https://draw.io/)
- [xmind](https://xmind.app/)
Many DFDs may be required for some systems. Maybe a DFD genreic for the entire system as well as other DFD for more specific sub-systems.
The DFD should provide a clear view of 
 - trust boundaries
 - data flows
 - data stores
 - processes
 - the external entities which may interact with the system
#### What can go wrong?
After the system has been modeled, the following wuestion is " *What can go wrong* "
This step should focus on identifying and ranking threats whithin the context of the specific system being evaluated. To identify and evaluate threats, could be use the STRIDE method. 

| Threat Category             | Violates          | Examples                                                                                                    |
| --------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------------- |
| **S**poofing                | Authenticity      | An attacker steals the authentication token of a legitimate user and uses it to impersonate the user.       |
| **T**ampering               | Integrity         | An attacker abuses the application to perform unintended updates to a database.                             |
| **R**epudiation             | Non-repudiability | An attacker manipulates logs to cover their actions.                                                        |
| **I**nformation Disclosure  | Confidentiality   | An attacker extract data from a database containing user account info.                                      |
| **D**enial of Service       | Availability      | An attacker locks a legitimate user out of their account by performing many failed authentication attempts. |
| **E**levation of Privileges | Authorization     | An attacker tampers with a JWT to change their role.                                                        |
The STRIDE is incorporated into [OWASP's Threat Dragon](https://github.com/OWASP/threat-dragon) and [Microsoft's Threat Modeling Tool](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool)
Identify all the possible threats and in some cases may be also be ranked

##### Response and mitigations
Once identified the possible threats, it's time to ask *what are we going to do about it*?. For each threat must have a response. 
The response to the threat could be one of the following:

- **Mitigate:** Take action to reduce the likelihood that the threat will materialize.
- **Eliminate:** Simply remove the feature or component that is causing the threat.
- **Transfer:** Shift responsibility to another entity such as the customer.
- **Accept:** Do not mitigate, eliminate, or transfer the risk because none of the above options are acceptable given business requirements or constraints.

#### Review and Validation
On this step is answered the question *"did we do a good enough job"?* after there's a response for the threat we may ask us the following questions:
- Does the DFD reflects the system?
- Have all threads being identified?
- All identified thread have response?
- Those response reduce the thread's impact