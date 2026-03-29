## To store user files (like avatar, etc) we can use:

- Google Drive - using API or S3
- Server + NGINX
- AWS S3
- Minio as S3-compatible self-hosted storage

## Storage Service should implement abstract fabric with methods

- AbstractStorageService
-- upload(file: File, public: boolean) -> str
-- upload_from_url(url: str, public: boolean, [headers: dict]) -> str // calls upload after downloading file from url
-- upload_from_base64(base64: str, public: boolean) -> str // calls upload after decoding base64
-- download(file_id: str) -> File
-- get_public_url(file_id: str) -> str
-- delete(file_id: str) -> bool