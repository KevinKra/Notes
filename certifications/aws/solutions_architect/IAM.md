# IAM

> "Identity Access Management" allows you to manage users and their level of access to the AWS console

- IAM is a **Global Service**.

## Root Account

- Email address used to sign up for AWS.
- Has **full admin access** to AWS.

#### How to secure the Root Account

- Enable MFA.
- Create Admin group for administrators, assign appropriate permissions to group.
- Create user accounts for your admins.
- Add users to the admin group.

### Policy Documents

- Written in **JSON**.
- Easiest approach for establishing uniform permissions is by attaching policies to groups and then assigning users to the groups.
- **Managed Policies** are policies created _and managed_ by AWS.
- Allow statements **DO NOT** override explicit Deny statements.

```
// policy example:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

## IAM Management

- **User**: an account bound to unique users.
- **Groups**: organizing users, ie. admins, developers, HR, ect.
- **Roles**: used by AWS services to grant access to other services. Example: S3 communicating with ECS.

IAM users are considered "permanent" because user credentials don't automatically rotate, making them "permanent" without manual human interaction.

You can create roles and attach users to them. **STS (Security Token Service)** allows the user to be attached to the role within the AWS console. The user(s) can then _temporarily assume the role_ to have access to the provided permissions provided by the role.

### Exam Tips:

- IAM is a global service.
- Access Key ID and Secret Access Keys are not the same as usernames and passwords.
- You only can view your Secret Access Key once, save it in a secure location.
- Always setup password rotations.
- IAM Federation, you can combine existing user accounts with AWS. For example, when you login using your PC (using Microsoft Active Directory), you can use the same credentials to log into AWS if you've setup federation.
- Identity Federation: uses SAML standard, which is Active Directory.

## Questions:

- What language are AWS policies written in?
- What is the easiest way to establish uniform policies across an organization?
- What are managed policies? What naming identifier do these policies share?
- Do allow statements override explicit deny statements?
- What is the structure of a generic policy?
- What are the three IAM entities? Explain each.
- IAM users are considered permanent, what does this mean?
- What can you use to create roles and attach them to users to grant them temporary permissions?
- How many times can a user view their secret access key? What has to be done if it's lost or forgotten?
- What is IAM Federation and what standard does it use?
