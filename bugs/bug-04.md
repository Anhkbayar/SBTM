# BUG-04: POST /api/books буруу body-г 400-өөр няцаахгүй, null body серверийг унагадаг

Орчин: Уншлагын жагсаалт v2.0, curl, Node.js v24.21.0, Linux 7.0.0-38-generic x86_64, 2026-10-21

Урьдчилсан нөхцөл: node app.js-ийг шинээр асаасан; анхны 3 ном.

## Дахин гаргах алхам

1. Аппыг шинээр асаана.
2. curl -i -X POST http://localhost:3000/api/books -H 'Content-Type: application/json' -d '{' хүсэлт явуулна.
3. Аппыг Ctrl+C-ээр зогсоож дахин асаана.
4. curl -i -X POST http://localhost:3000/api/books -H 'Content-Type: application/json' --data-binary 'null' хүсэлт явуулна.
5. Дараа нь curl -i http://localhost:3000/api/books гэж шалгана.

Хүлээгдэж байсан: Буруу JSON body-д HTTP 400 буцаж, апп ажилласаар байна; шинэ бичлэг үүсэхгүй.

Бодит үр дүн: { body-д HTTP 200 OK болон {"result":"OK"} буцна. null body-д curl Empty reply from server авч, node app.js exit 1-ээр унтарна; дараагийн GET холбогдож чадахгүй. Мөн хоосон body 201 Created болж хоосон бичлэг үүсгэсэн.

Ноцтой байдал: Critical — нэг API хүсэлт аппын процессыг бүхэлд нь зогсоож, бусад хэрэглэгчийн хүсэлтийг тасалдуулна.

Хавсралт: [буруу JSON-ийн бүтэн HTTP гаралт](../evidence/bug-04-curl.txt), [null body curl гаралт](../evidence/bug-04-null-curl.txt), [сервер унтарсны дараах GET](../evidence/bug-04-server-down.txt), [хоосон body-ийн response](../evidence/bug-04-empty-curl.txt), [үүссэн бичлэг](../evidence/bug-04-empty-books-after.txt)

Олдсон аялал: Saboteur (API түвшний довтолгоо)
