## **AWS Solutions Architect Notes - IAM**

**AWS Global Infrastructure**
- Regions
- Availability Zones
- Data Centers
- Edge Locations/Points of Presence

**How to choose an AWS Region**
- Compliance
  - Data governance and legal requirements
- Proximity
- Available Services
- Pricing

**AWS Availability Zones**
- Each region has many availability zones (average:3, min: 2, max: 6)

**IAM: Users & Groups**
- IAM = Identity and Access Management, Global Service
- The Least Privilege Principle: Don't give more permissions than a user needs
- IAM is a global service

**MFA**
- Multi-Factor Authentication
- MFA device options include:
  - Virtual MFA device
    - Google Authenticator
    - Authy
  - Universal 2nd Factor (U2F) Security Key
    - Physical Device
      - YubiKey
  - Hardware Key Fob MFA device
    
**AWS CloudShell: Region Availability**

Currently, AWS CloudShell is available in the following AWS Regions:

- US East (Ohio)
- US East (N. Virginia)
- US West (Oregon)
- Asia Pacific (Mumbai)
- Asia Pacific (Sydney)
- Asia Pacific (Tokyo)
- Europe (Frankfurt)
- Europe (Ireland)

**IAM Roles**

IAM Roles: are just roles that are set to AWS services that will assign permissions

**IAM Security Tools**
- IAM Credentials Report(account-level)
  - a report that lists all your accounts users and the status of their various credentials
- IAM Access Advisor(user-level)
  - Access advisor shows the service permissions granted to a user and when those services that were last accessed.
  - You can use this information to revise your policies.

**Questions**
1. An IAM policy consists of one or more statements. A statement in an IAM Policy consists of the following, EXCEPT:
   - Version
     - A statement in an IAM Policy consists of Sid, Effect, Principal, Action, Resource, and Condition. 
       Version is part of the IAM Policy itself, not the statement.
2. Which of the following is an IAM Security Tool?
    - IAM Credentials Report
3. What are IAM Policies?
    - JSON documents that set permissions for making requests to AWS services, and can be used by IAM Users, User Groups, 
      and IAM Roles
4. Which principle should you apply regarding IAM Permissions?
    - The Least Grant Principle
5. IAM User Groups can contain IAM Users and other User Groups.
    - False
      - IAM User Groups can only contain IAM Users
6. According to the AWS Shared Responsibility Model, which of the following is AWS responsibility?
   - AWS Infrastructure 
    