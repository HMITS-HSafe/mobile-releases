# mobile-releases

Repo public dùng riêng để publish bản cập nhật HSafe ngoài Play Store. Không chứa source code.

## Cách hoạt động

App đọc file [`versions.json`](./versions.json) ở nhánh `main` (qua
`raw.githubusercontent.com/HMITS-HSafe/mobile-releases/main/versions.json`) mỗi khi mở màn
Cài đặt, key theo `companyId` (mỗi khách hàng build 1 flavor riêng — xem `COMPANY_ID` trong
`app/build.gradle.kts` của repo `mobile`).

```json
{
  "<companyId>": {
    "latestVersion": "1.2.0",
    "minSupportedVersion": "1.1.0",
    "downloadUrl": "https://github.com/HMITS-HSafe/mobile-releases/releases/download/v1.2.0-<companyId>/hsafe-<companyId>-1.2.0.apk",
    "releaseNotes": "Mô tả ngắn thay đổi trong bản này"
  }
}
```

- `latestVersion` — bản mới nhất. Nếu app đang cài cũ hơn: hiện card gợi ý update trong Cài đặt
  (không bắt buộc).
- `minSupportedVersion` — bản tối thiểu còn được hỗ trợ. Nếu app đang cài **cũ hơn** mức này:
  hiện popup **bắt buộc** cập nhật, không cho tắt, chặn dùng app cho tới khi cập nhật.
- `downloadUrl` — link mở khi user bấm nút cập nhật (thường trỏ tới release có đính kèm APK).

## Quy trình release 1 bản mới

1. Build APK cho company tương ứng (`./gradlew assemble<CompanyFlavor>Release`).
2. Tạo release + tag trên repo này (vd `v1.2.0-default`), đính kèm file APK.
3. Cập nhật `versions.json`: sửa `latestVersion`/`downloadUrl`/`releaseNotes` cho đúng
   `companyId`. Chỉ sửa `minSupportedVersion` khi thật sự muốn ép user cũ phải update ngay.
4. Commit + push lên `main`.
