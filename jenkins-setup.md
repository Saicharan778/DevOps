# Jenkins Installation on Ubuntu EC2

## Step 1 — Install Java 21

```bash
sudo apt update
sudo apt install -y fontconfig openjdk-21-jre
```

Verify Java:

```bash
java -version
```

---

## Step 2 — Download the Current Jenkins Signing Key

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

---

## Step 3 — Add the Jenkins LTS Repository

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

---

## Step 4 — Update the Package List

```bash
sudo apt update
```

---

## Step 5 — Install Jenkins

```bash
sudo apt install -y jenkins
```

---

## Step 6 — Enable Jenkins

This makes Jenkins start automatically when the EC2 instance boots.

```bash
sudo systemctl enable jenkins
```

---

## Step 7 — Start Jenkins

```bash
sudo systemctl start jenkins
```

---

## Step 8 — Check Jenkins Status

```bash
sudo systemctl status jenkins
```

You should see:

```text
Active: active (running)
```

---

## Get the Jenkins Initial Password

After Jenkins is running:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password displayed in the terminal.

---

## Access Jenkins

Get your EC2 public IP:

```bash
curl http://checkip.amazonaws.com
```

Then open this in your browser:

```text
http://YOUR_EC2_PUBLIC_IP:8080
```

Example:

```text
http://13.234.123.45:8080
```

Make sure your AWS EC2 Security Group allows **TCP port 8080**.

---

## Important

Do **not** use the old Jenkins signing key:

```text
jenkins.io-2023.key
```

Use the current key:

```text
jenkins.io-2026.key
```
