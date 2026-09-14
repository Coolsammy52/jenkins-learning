1. Launched EC2 (c7i-flex.large, free tier), connected via SSH

2. Installed Java 17, then had to add Java 21 — Jenkins LTS dropped Java 17 support

3. Added Jenkins apt repo — hit NO_PUBKEY error because the tutorial's signing key (2023) was retired; fixed using the current 2026 key

4. systemctl start jenkins failed — diagnosed via journalctl -u jenkins, found the real Java version error buried in the logs

5. Opened port 8080 in the Security Group, then hit a timeout — traced it through Jenkins itself (curl localhost:8080 → 403, confirmed Jenkins was fine), ufw (inactive), and finally found the EC2 public IP had changed after stop/start

6. Completed setup wizard, created admin user, reached dashboard
