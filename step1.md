#### Apa Itu Namespace

Namespace adalah cara untuk mengelompokkan dan mengelola sumber daya di dalam kluster Kubernetes. Namespace digunakan untuk mengisolasi dan membagi lingkungan kerja dalam kluster yang sama.

Namespace dalam Kubernetes bisa diibaratkan seperti ruangan dalam sebuah gedung perkantoran. Gedung perkantoran tersebut adalah kluster Kubernetes, dan setiap ruangan di dalam gedung mewakili sebuah Namespace.

Setiap ruangan atau Namespace memiliki resource dan peralatan yang diperlukan oleh tim yang berbeda untuk menjalankan pekerjaannya. Misalnya, satu ruangan digunakan oleh tim pengembangan, ruangan lain oleh tim QA, dan lainnya oleh tim pemasaran. Ruangan ini membantu mengelola dan mengisolasi resource, sehingga tim-tim ini dapat bekerja secara efisien tanpa mengganggu atau mempengaruhi pekerjaan tim lain.

Secara default, Kubernetes memiliki tiga Namespace:

- `default`: untuk resource yang tidak memiliki Namespace
- `kube-system`: untuk resource yang dibuat oleh sistem Kubernetes
- `kube-public`: untuk resource yang dapat diakses oleh semua pengguna

Untuk melihat Namespace yang ada, jalankan perintah berikut:

```{.bash .copy}
kubectl get namespaces
```
