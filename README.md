# Axial Motion

Corporate site for [axialmotion.com](https://axialmotion.com), hosted on Google Cloud Run with Cloud DNS.

## Stack

- Static HTML/CSS in `public/`
- Nginx container on Cloud Run (`us-central1`)
- Cloud DNS zone `axialmotion-com` in GCP project `axialmotion-web`

## Deploy

```bash
gcloud run deploy axialmotion \
  --source=. \
  --region=us-central1 \
  --allow-unauthenticated \
  --project=axialmotion-web
```

## DNS

Registrar is Bluehost. DNS currently lived at DreamHost (`NS1.DREAMHOST.COM` / `NS2.DREAMHOST.COM`). Cloud DNS in project `axialmotion-web` is ready.

In Bluehost: Domains → axialmotion.com → Nameservers → **Change nameservers** to:

- `ns-cloud-d1.googledomains.com`
- `ns-cloud-d2.googledomains.com`
- `ns-cloud-d3.googledomains.com`
- `ns-cloud-d4.googledomains.com`

After nameservers propagate, verify the domain in [Google Search Console](https://search.google.com/search-console) as `anmabot@gmail.com`, then map Cloud Run:

```bash
gcloud beta run domain-mappings create \
  --service=axialmotion \
  --domain=axialmotion.com \
  --region=us-central1 \
  --project=axialmotion-web

gcloud beta run domain-mappings create \
  --service=axialmotion \
  --domain=www.axialmotion.com \
  --region=us-central1 \
  --project=axialmotion-web
```

Apex A/AAAA and `www` CNAME records are already in Cloud DNS.

Live preview (before custom domain): https://axialmotion-177107826278.us-central1.run.app
