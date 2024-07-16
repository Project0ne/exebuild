import sys
import os
import requests
import csv
import xml.etree.ElementTree as ET
from tqdm import tqdm
from concurrent.futures import ThreadPoolExecutor, as_completed

def check_remote_status(url):
    response = requests.get(url)
    response.raise_for_status()  # 确保请求成功
    content = response.content.decode('utf-8').strip().lower()
    return content == 'true'

# 检查远程CSV文件的值
csv_url = 'https://gitee.com/Project0ne/cdn/raw/master/src/1.csv'
if not check_remote_status(csv_url):
    print("Script is disabled.")
    sys.exit(1)

# 检查是否提供了XML文件名
if len(sys.argv) != 2:
    print("Usage: python process_xml.py <xml_filename>")
    sys.exit(1)

xml_filename = sys.argv[1]

# 解析XML文件
tree = ET.parse(xml_filename)
root = tree.getroot()

# 定义CSV文件的列名
csv_columns = ['ITEMID', 'COLOR', 'MINQTY', 'PRODUCT_ID', 'COLOR_ID']

# 获取ITEM的总数以便显示进度条
items = root.findall('ITEM')
total_items = len(items)

# 生成输出CSV文件名
output_csv = os.path.splitext(xml_filename)[0] + '.csv'

def fetch_product_id(item_id):
    item_api_url = f'https://gobricks.cn/frontend/v1/item/filter?type=all&page=1&order_direction=desc&variety=all&grouping=product_id&hasInventory=YES&caption={item_id}'
    item_response = requests.get(item_api_url).json()
    for row in item_response['rows']:
        if item_id in row['ldraw_no'].split(','):
            return f"GDS-{row['product_id']}"
    return None

def fetch_color_id(color):
    color_api_url = 'https://gobricks.cn/frontend/v1/item/getSingleConfig?key=shopItemColor'
    color_response = requests.get(color_api_url).json()
    for color_data in color_response:
        if color_data['ldraw_color_id'] == color:
            return color_data['id']
    return None

def process_item(item):
    item_id = item.find('ITEMID').text
    color = item.find('COLOR').text
    min_qty = item.find('MINQTY').text

    product_id = fetch_product_id(item_id)
    color_id = fetch_color_id(color)

    return {
        'ITEMID': item_id,
        'COLOR': color,
        'MINQTY': min_qty,
        'PRODUCT_ID': product_id,
        'COLOR_ID': color_id
    }

# 使用多线程处理
with ThreadPoolExecutor(max_workers=10) as executor:
    futures = [executor.submit(process_item, item) for item in items]
    results = []
    for future in tqdm(as_completed(futures), total=total_items, desc="Processing items"):
        results.append(future.result())

# 写入CSV文件
with open(output_csv, 'w', newline='') as csvfile:
    writer = csv.DictWriter(csvfile, fieldnames=csv_columns)
    writer.writeheader()
    writer.writerows(results)

print(f"数据已成功保存到 {output_csv} 文件中。")
