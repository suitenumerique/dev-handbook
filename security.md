# Security at La Suite numérique

Here are few hints and requirements regarding the security in La Suite's projects.

## GitHub

- [ ] Commit signing is mandatory, preferably using an external component (e.g., Yubikey);
- [ ] As a repository administrator, you must enforce 2FA for all members with repository rights;
- [ ] As a non-administrator repository user, you must enable 2FA on your account;
- [ ] Deploy manually to production: do not perform automatic deployments to production from the code repository (GitHub);

## Developer

- [ ] Use a password manager for all secrets, do not write your passwords in plain text 
      (on paper, or even in a file); If this is not possible, you must split the 
      information across multiple channels.
- [ ] Choose long passwords (at least 20 characters), complex and unique for each service;
- [ ] Create one account per person and avoid sharing accounts;
- [ ] Encrypt your storage devices;
- [ ] Use non-versioned environment variables, different between local, staging, and production;
- [ ] Develop in a containerized environment to protect the development machine from malicious dependencies;
