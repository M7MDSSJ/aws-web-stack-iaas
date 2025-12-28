# SSH Key Pair Management

This document explains how SSH key pairs are created and used for secure access to EC2 instances.

---

## Key Pair Creation

- Created via AWS Management Console or AWS CLI  
- AWS stores the **public key**, and the user downloads the **private key (.pem file)**  
- The private key file must be securely stored and its permissions set to `chmod 400`

---

## Using the Key Pair

- The private key is used to SSH into EC2 instances:  

    ssh -i /path/to/key.pem ubuntu@<ec2-instance-public-ip>

- Usernames depend on the AMI:
  - Ubuntu: `ubuntu`
  - Amazon Linux: `ec2-user`

---

## Security Best Practices

- Do **not** share private keys  
- Use different key pairs per environment/project  
- Rotate keys regularly  
- Disable password authentication on instances  
- Limit SSH access via security groups (source IP restrictions)

---

## Notes

- Key pairs are assigned to EC2 instances at launch  
- Access is only allowed through configured security groups  
- Lost private keys require instance replacement or key injection

---

## Summary

SSH key pairs provide secure, password-less access to EC2 instances and are essential for instance management and validation.
