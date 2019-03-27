## Описание

Репозиторий по деплою Ingress Controller на основе https://kubernetes.github.io/ingress-nginx/
с использованием [k8s-handle](https://github.com/2gis/k8s-handle)

Для предотвращения несанкционированного доступа через внешний Ingress Controller к ресурсам внутренней сети,
куда доступа быть не должно, реализована поддержка NetworkPolicy.

#### Изменения в деплое

Все изменения происходят по правилам необходимым для [k8s-handle](https://github.com/2gis/k8s-handle#configuration-structure)

#### Настройка безопасности ingress controller (NetworkPolicy)

Для настройки безопасности существуют кастомизируемые правила NetworkPolicy.
Переменные, которые можно использовать для их кастомизации:

* `network_policy_egress` - dict с правилами для разрешения исходящего трафика с ingress controller
  * `cidrs` - array с подсетями, куда можно будет ходить с ingress controller
  * `selectors` - array dict'ов с лейблами Namespace'а и POD'а, на которые будет разрешен исходящий трафик с ingress controller
    * `namespaceSelector` - string с указанием лейбла Namespace'а, в формате `"key: value"`
    * `podSelector` - string с указанием лейбла POD'а, в формате `"key: value"`
* `network_policy_ingress` - dict с правилами для разрешения входящего трафика на ingress controller
  * `cidrs` - array с подсетями, откуда можно будет обращаться к ingress controller
  * `selectors` - array dict'ов с лейблами Namespace'а и POD'а, с которых будет разрешен входящий трафик на ingress controller
    * `namespaceSelector` - string с указанием лейбла Namespace'а, в формате `"key: value"`
    * `podSelector` - string с указанием лейбла POD'а, в формате `"key: value"`

Если какая-либо из переменных(`network_policy_egress` или `network_policy_ingress`) не задана, значит в этом направлении разрешен весь трафик.

#### Пример использования

```
$ k8s-handle deploy -s public
$ k8s-handle deploy -s secure
```

#### Дополнительные материалы

* В репозитории [example-apps](https://github.com/Z8g7dege570HC05u/example-apps) описаны простейшие сервисы для проверки
работы ingress controller и networkPolicy заданных в исходном config.yaml
* В качестве контроллера для networkPolicy выступает [kube-router](https://github.com/Z8g7dege570HC05u/kube-router)
