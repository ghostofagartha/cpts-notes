**AWS S3 / Azure Blobs / GCP Cloud Storage**
- Often misconfigured = accessible without auth
- Stored in DNS records for employee access

***Finding Cloud Storage:***

1. **Google Dorks**
   - inurl:amazonaws.com (AWS)
   - inurl:blob.core.windows.net (Azure)
   - inurl:storage.googleapis.com (GCP)
   - Example: intext:[company] inurl:amazonaws.com

1. **DNS List Method**
   - Get subdomains: for i in $(cat subdomainlist);do host $i | grep "has address" | cut -d" " -f1,4;done
   - Look for s3-website-* or blob.core.windows.net entries

1. **Domain.Glass**
   - Aggregator of company infrastructure info
   - Shows security assessment (Cloudflare status, etc.)

1. **GrayHatWarfare**
   - Searchable database of exposed cloud storage
   - Filter by cloud provider + file type
   - Often finds leaked SSH keys (id_rsa)

***KEY FINDING: SSH Private Keys***
- Leaked in misconfigured buckets
- Anyone with key can access company machines
- Critical vulnerability