# Identity Federation

Identity Federation is used to provide users/entities outside of aws permissions to access aws resources.

> IAM supports IdPs that are compatible with OpenID Connect (OIDC) or SAML 2.0 (Security Assertion Markup Language 2.0)

- Web Identity Federation: Useful for mobile app use cases. Use well known IDP such as Google or Facebook or Amazon for sign-in and get token. Exchange this token for short-term credentials that allow aws services access.
    - Cognito is best suited for Web Identity Federation. 
    - If you don't use Amazon Cognito, then you must write code that interacts with a web IdP, such as Facebook, and then calls the AssumeRoleWithWebIdentity API to trade the authentication token you get from those IdPs for AWS temporary security credentials.  
- SAML 2.0 Federation: This feature enables federated single sign-on (SSO), so users can log into the AWS Management Console or call the AWS API operations without you having to create an IAM user for everyone in your organization.
    - AWS SSO is best suited for SMAL 2.0 Federation   

## Federation Services

### SAML2.0 Federation:

- Integration with Microsoft ADFS (Active Directory Federation Services)
- Under the hood: uses STS AssumeRolewithSAML
- Involves creation of SAML Identity Provider in IAM
- Can provide *trust policy* and *permission policies* as usual. The principal here would be **saml-provider**

SAML Assertion Workflow:

![image](https://user-images.githubusercontent.com/15995686/171585160-5532136f-b382-471a-81ad-cae41a16f58e.png)

Example Trust Policy:
```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"Federated": "arn:aws:iam::account-id:saml-provider/ExampleOrgSSOProvider"},
    "Action": "sts:AssumeRoleWithSAML",
    "Condition": {"StringEquals": {
      "saml:edupersonorgdn": "ExampleOrg",
      "saml:aud": "https://signin.aws.amazon.com/saml"
    }}
  }]
}
```
   
### AWS SSO:

- AWS SSO Federation is the new and managed way. Underlying uses SAML 2.0 Federation.

### Custom Identity Broker Application

- Use only if Identity provider is NOT compatible with SAML 2.0
- *Disadvantage:* The identity broker must determine appropriate IAM role (with SAML 2.0, the assertion has a role)
- GetFederationToken

### Web Identity Federation - without Cognito (not recommended)

- Uses AssumeRoleWithWebIdentity 

### Web Identity Federation - with Cognito

- Preferred by aws.
- Can create Roles within Cognito with least privileges.
- MFA, Data sync and anonymous users support.

### Web Identity Federation - IAM Policy

- After identity federation, can identify a user in IAM Policy through variables.
- Example: cognito- identity.amazonaws.com:sub


## Directory Services

## Best Practices

## References
