import requests
import json
import base64  

class TrendyolStockManager:
    def __init__(self, api_key, api_secret, supplier_id):
        self.base_url = "https://api.trendyol.com/sapigw/"
        
        auth_string = f"{api_key}:{api_secret}"
        encoded_auth = base64.b64encode(auth_string.encode("utf-8")).decode("utf-8")

        self.headers = {
            "Content-Type": "application/json",
            "Authorization": f"Basic {encoded_auth}"  
        }
        self.supplier_id = supplier_id

    def update_stock(self, product_barcode, new_quantity, sale_price, list_price):
        
        url = f"{self.base_url}suppliers/{self.supplier_id}/products/price-and-inventory"

        stock_data = {
            "items": [{
                "barcode": product_barcode,   
                "quantity": new_quantity,    
                "salePrice": sale_price,     
                "listPrice": list_price      
            }]
        }

        
        response = requests.post(url, json=stock_data, headers=self.headers)

        
        if response.status_code == 200:
            print("Stok ve fiyat bilgileri başarıyla güncellendi!")
            return response.json()
        else:
            
            print("Hata:", response.status_code, response.text)
            return None


if __name__ == "__main__":

    API_KEY = "bj3R2QdrpDMdKHof5ugr"
    API_SECRET = "6Y93tP6xCi9vZDyWDdfy"
    SUPPLIER_ID = "1123575"


    manager = TrendyolStockManager(API_KEY, API_SECRET, SUPPLIER_ID)


    barcode = "87542867"
    new_stock = 100
    sale_price = 299.99
    list_price = 399.99

    
    manager.update_stock(barcode, new_stock, sale_price, list_price)

