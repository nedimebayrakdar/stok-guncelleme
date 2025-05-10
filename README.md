# stok-guncelleme

import time
import hmac
import hashlib
import base64
import requests


api_key = "bj3R2QdrpDMdKHof5ugr"
api_secret = "6Y93tP6xCi9vZDyWDdfy"
supplier_id = "1123575"


timestamp = str(int(time.time() * 1000))


message = api_key + timestamp
signature = base64.b64encode(
    hmac.new(api_secret.encode('utf-8'), message.encode('utf-8'), hashlib.sha256).digest()
).decode()


headers = {
    "Content-Type": "application/json",
    "Authorization": api_key,
    "x-trendyol-authentication": f"{api_key}:{signature}:{timestamp}"
}


url = f"https://api.trendyol.com/sapigw/suppliers/{supplier_id}/products?approved=false&page=0&size=50"
response = requests.get(url, headers=headers)

if response.status_code == 200:
    print(" Bağlantı başarılı, ürünler alındı.")
    print(response.json())
else:
    print(f" Hata: {response.status_code}")
    print(response.text)
