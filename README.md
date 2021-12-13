# 14.2 Синхронизация секретов с внешними сервисами. Vault   
- Запустил модуль Vault конфигураций [vault-pod.yml](https://github.com/Kostromin-Mixa/kube_secret-14.2/blob/main/vault-pod.yml)    
kubectl apply -f 14.2/vault-pod.yml   
- Запустил второй модуль для использования в качестве клиента   
kubectl run -i --tty fedora --image=fedora --restart=Never -- sh

![3](https://user-images.githubusercontent.com/78191008/145844233-19e096db-a258-4a50-9f67-c43c3b8249d8.jpg)

- Установил сервис для доступности внутри крастера к Vault [service.yml](https://github.com/Kostromin-Mixa/kube_secret-14.2/blob/main/service.yml)    
- Получил значение внутреннего IP пода 14.2-netology-vault    
kubectl get pod 14.2-netology-vault -o json | jq -c '.status.podIPs'

![2](https://user-images.githubusercontent.com/78191008/145844448-5ad371cc-9609-40a7-8c6f-a5d7492446ed.jpg)

- Во втором модуле fedora установил дополнительные пакеты:   
dnf -y install pip    
pip install hvac    

- Запустил интепретатор Python и выполнил следующий код:    

import hvac   
client = hvac.Client(   
    url='http://172.17.0.3:8200',   
    token='aiphohTaa0eeHei'   
)   
client.is_authenticated()   

Пишем секрет   
client.secrets.kv.v2.create_or_update_secret(   
    path='hvac',   
    secret=dict(netology='Big secret!!!'),   
)   

Читаем секрет   
client.secrets.kv.v2.read_secret_version(   
    path='hvac',   
)   
- Проверяем в веб браузере Vault    
 ![1](https://user-images.githubusercontent.com/78191008/145847672-8f08877d-f877-4988-91cc-c2f32f52f451.jpg)
