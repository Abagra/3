
echo "# KT-test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/KsuStahanova/KT-test.git
git push -u origin main


import requests
import pprint

class BaseRequest:
def init(self, base_url):
self.base_url = base_url
# set headers, authorisation etc

python
Copy code
def _request(self, url, request_type, data=None, expected_error=False):
    stop_flag = False
    while not stop_flag:
        if request_type == 'GET':
            response = requests.get(url)
        elif request_type == 'POST':
            response = requests.post(url, data=data)
        else:
            response = requests.delete(url)

        if not expected_error and response.status_code == 200:
            stop_flag = True
        elif expected_error:
            stop_flag = True

    # log part
    pprint.pprint(f'{request_type} example')
    pprint.pprint(response.url)
    pprint.pprint(response.status_code)
    pprint.pprint(response.reason)
    pprint.pprint(response.text)
    pprint.pprint(response.json())
    pprint.pprint('**********')
    return response

def get(self, endpoint, endpoint_id, expected_error=False):
    url = f'{self.base_url}/{endpoint}/{endpoint_id}'
    response = self._request(url, 'GET', expected_error=expected_error)
    return response.json()

def post(self, endpoint, endpoint_id, body):
    url = f'{self.base_url}/{endpoint}/{endpoint_id}'
    response = self._request(url, 'POST', data=body)
    return response.json()['message']

def delete(self, endpoint, endpoint_id):
    url = f'{self.base_url}/{endpoint}/{endpoint_id}'
    response = self._request(url, 'DELETE')
    return response.json()['message']
BASE_URL_PETSTORE = 'https://petstore.swagger.io/v2'
base_request = BaseRequest(BASE_URL_PETSTORE)

User requests
user_info = base_request.get('user', '1')
pprint.pprint(user_info)

user_data = {
"id": 2,
"username": "john_doe",
"firstName": "John",
"lastName": "Doe",
"email": "john.doe@example.com",
"password": "password",
"phone": "1234567890",
"userStatus": 1
}
response_message = base_request.post('user', '2', user_data)
pprint.pprint(response_message)

deleted_id = base_request.delete('user', '3')
deleted_user_info = base_request.get('user', deleted_id, expected_error=True)
pprint.pprint(deleted_user_info)

Store requests
store_info = base_request.get('store', '1')
pprint.pprint(store_info)

inventory_data = {
"item_id": 1,
"quantity": 10,
"status": "available"
}
response_message = base_request.post('store', '2', inventory_data)
pprint.pprint(response_message)

deleted_id = base_request.delete('store', '3')
deleted_store_info = base_request.get('store', deleted_id, expected_error=True)
pprint.pprint(deleted_store_info)
