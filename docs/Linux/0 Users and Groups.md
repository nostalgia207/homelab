#### 1 - Create the user

```bash
sudo adduser test_user
```

#### 2 - Create group and add user to it

```bash
sudo groupadd devops
sudo usermod -aG devops test_user
```

#### 3 - Add user to docker group

```bash
sudo usermod -aG docker test_user
```

> [!warning] Docker group membership is effectively root-equivalent - a user in this group can mount the host's `/` into a container and modify anything on the system. Treat it like sudo access.

#### 4 - Verify

```bash
id test_user
groups test_user
```

> [!tip] To check all members of a given group instead of just one user: `getent group docker`

> [!note] Group membership changes don't apply to an already-open session. Log out/in (or reconnect via SSH) before testing.

---