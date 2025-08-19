# 📖 Ready to use LLDAP Compose Stack

Docker Compose configuration to deploy [**LLDAP**](https://github.com/lldap/lldap) with Docker and Portainer in a custom network (`cosmos`) with a static IP, with only the WebUI port exposed.

---

# ⚡ How to Start : 

- Clone the repo
- In the cloned repo, paste this in the CLI, it will generate a 32 character long chain foe each in the .env file :

```bash
sed -i "s|^LLDAP_JWT_SECRET=.*|LLDAP_JWT_SECRET=$(openssl rand -base64 32 | tr -d '\r\n')|" .env
sed -i "s|^LLDAP_KEY_SEED=.*|LLDAP_KEY_SEED=$(openssl rand -base64 32 | tr -d '\r\n')|" .env
```
- The following steps are to be done in the docker-compose.yml file :
- Change the admin PW with yours with at least 8 characters
- Paste the LLDAP_JWT_SECRET and LLDAP_KEY_SEED keys of the .env file in the compose
- Modify your Time Zone (TZ)

---

## 🐳 Portainer 

Here's how you deploy the compose with portainer : 

- Stacks -> New Stack -> Name it, load your compose file, hit deploy, Enjoy the GUI on port 17170 !

---

## ✅ Conclusion
You have now :

- A stable LLDAP container with a static IP (192.168.95.60)
- An LDAP domain : dc=cosmos,dc=corp (you can change it in the compose)
- An Admin PW
- Data persistance via a Docker volume (lldap_data) → clean and secure
- Ready to use with **Authentik**(https://github.com/BangSkyline/Helios)

---

### How to save the data (optionnal)

```bash
docker volume inspect lldap_data
```
### Save the volume

```bash
docker run --rm -v lldap_data:/data -v $(pwd):/backup alpine tar czf /backup/lldap-backup-$(date +%F).tar.gz -C /data .
```
### Restore the volume (delete it first to avoid conflicts)
```bash
docker volume rm lldap_data
docker volume create lldap_data
docker run --rm -v lldap_data:/data -v $(pwd):/backup alpine tar xzf /backup/lldap-backup-2025-04-05.tar.gz -C /data
```
