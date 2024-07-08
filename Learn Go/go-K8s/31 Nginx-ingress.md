```shell
apiVersion: networking.k8s.io/v1

kind: Ingress

metadata:

  name: shopping-list-ingress

  annotations:

    nginx.ingress.kubernetes.io/ssl-redirect: "false"

spec:

  ingressClassName: nginx

  rules:

  - http:

      paths:

        - path: /api/v1/shopping-items

          pathType: Prefix

          backend:

            service:

              name: shopping-list-api

              port:

                number: 8081

        - path: /

          pathType: Prefix

          backend:

            service:

              name: shopping-list-front-end

              port:

                number: 8080
```

- 定义服务转发


- 为什么引入ingress
	- 七层负载
	- 南北向流量管理
	- 优化发布策略
- 最佳实践
[Kubernetes Ingress 最佳实践_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1KF411T7oA/?spm_id_from=333.788.recommend_more_video.0&vd_source=dfa876815ff4fa974172466e667cfd18)
- 基础ingress不支持header分流或cookie分流
	- 用istio或ingress-nginx
