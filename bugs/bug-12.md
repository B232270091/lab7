# BUG-12: Дутуу JSON-тэй POST хүсэлтийг API амжилттай гэж хариулдаг

**Орчин** Уншлагын жагсаалт v2.0, Microsoft Edge 158.0.4370.33, Windows 11, curl.exe, 2026-11-25.

**Урьдчилсан нөхцөл** `node app.js`-ийг шинээр асаасан, жагсаалт анхны 3 номтой.

## Дахин гаргах алхам

1. PowerShell дээр `malformed.json` файл үүсгэнэ. Файлд зөвхөн `{` тэмдэгт оруулна.

   ```powershell
   Set-Content -LiteralPath malformed.json -Value '{' -Encoding ASCII -NoNewline
   ```

2. `Content-Type`-ыг JSON гэж өгөөд тухайн файлыг `POST /api/books` хүсэлтээр илгээнэ.

   ```powershell
   curl.exe -i -X POST http://localhost:3000/api/books -H "Content-Type: application/json" --data-binary "@malformed.json"
   ```

3. Хариуны эхний мөр, хариуны биетийг шалгана.

**Хүлээгдэж байсан** JSON задрах боломжгүй тул API HTTP `400 Bad Request` буцаана.

**Бодит үр дүн** API `HTTP/1.1 200 OK` болон `{"result":"OK"}` хариулсан. Дараах `GET /api/books` хүсэлтэд анхны 3 ном л байсан.

**Ноцтой байдал** Major. Буруу хүсэлтийг клиент амжилттай гэж хүлээн аваад алдааг таних боломжгүй болно.

**Олдсон аялал** Saboteur

Хэрэв Ubuntu дээр давтах бол файлыг `printf '{' > malformed.json` гэж үүсгээд ижил хүсэлтэд `curl.exe`-ийн оронд `curl` гэж ажиллуулна.

## Хавсралт

[curl -i хүсэлтийн бүтэн хариу](../evidence/bug-12-curl.txt)
