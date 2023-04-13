#### Menggunakan Namespace

Pertama mari kita lihat semua resource yang ada dalam kubernetes

```{.bash .copy}
kubectl get all --all-namespaces
```

```{.bash}
NAMESPACE            NAME                                             READY   STATUS    RESTARTS      AGE
kube-system          pod/coredns-787d4945fb-42d5r                     1/1     Running   1 (11m ago)   2d5h
kube-system          pod/coredns-787d4945fb-hxxws                     1/1     Running   1 (11m ago)   2d5h
kube-system          pod/etcd-kind-control-plane                      1/1     Running   1 (11m ago)   2d5h
kube-system          pod/kindnet-5kwl5                                1/1     Running   1 (11m ago)   2d5h
kube-system          pod/kube-apiserver-kind-control-plane            1/1     Running   1 (11m ago)   2d5h
kube-system          pod/kube-controller-manager-kind-control-plane   1/1     Running   1 (11m ago)   2d5h
kube-system          pod/kube-proxy-9ghlg                             1/1     Running   1 (11m ago)   2d5h
kube-system          pod/kube-scheduler-kind-control-plane            1/1     Running   1 (11m ago)   2d5h
local-path-storage   pod/local-path-provisioner-75f5b54ffd-dgntk      1/1     Running   2 (11m ago)   2d5h

NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  2d5h
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   2d5h

NAMESPACE     NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   daemonset.apps/kindnet      1         1         1       1            1           kubernetes.io/os=linux   2d5h
kube-system   daemonset.apps/kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   2d5h

NAMESPACE            NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system          deployment.apps/coredns                  2/2     2            2           2d5h
local-path-storage   deployment.apps/local-path-provisioner   1/1     1            1           2d5h

NAMESPACE            NAME                                                DESIRED   CURRENT   READY   AGE
kube-system          replicaset.apps/coredns-787d4945fb                  2         2         2       2d5h
local-path-storage   replicaset.apps/local-path-provisioner-75f5b54ffd   1         1         1       2d5h
```

Perhatikan kolom `NAMESPACE`, umumnya resource yang ada dalam kubernetes memiliki namespace atau ditempatkan dalam namespace tertentu baik yang ditentukan oleh user ataupun tidak.

Resource yang tidak memiliki namespace saat dibuat akan secara otomatis dimasukkan ke dalam namespace default.

Ketika menggunakan `kubectl` tanpa menentukan namespace, maka perintah tersebut akan dieksekusi dalam namespace default. Jalankan perintah dibawah

```{.bash .copy}
kubectl get pods
```

Perintah ini tidak akan menampilkan apapun karena saat ini belum ada resource pada namespace `default`. Sekarang coba jalankan perintah yang sama dengan tambahan parameter namespace

```{.bash .copy}
kubectl get pods --namespace kube-system
```

```{.bash}
NAME                                         READY   STATUS    RESTARTS      AGE
coredns-787d4945fb-42d5r                     1/1     Running   1 (15m ago)   2d5h
coredns-787d4945fb-hxxws                     1/1     Running   1 (15m ago)   2d5h
etcd-kind-control-plane                      1/1     Running   1 (15m ago)   2d5h
kindnet-5kwl5                                1/1     Running   1 (15m ago)   2d5h
kube-apiserver-kind-control-plane            1/1     Running   1 (15m ago)   2d5h
kube-controller-manager-kind-control-plane   1/1     Running   1 (15m ago)   2d5h
kube-proxy-9ghlg                             1/1     Running   1 (15m ago)   2d5h
kube-scheduler-kind-control-plane            1/1     Running   1 (15m ago)   2d5h
```

Kita akan mendapati terdapat resource yang berjalan pada namespace `kube-system`.

#### Membuat Namespace

Untuk membuat namespace baru, kita cukup jalankan perintah `create namespace` seperti dibawah

```{.bash .copy}
kubectl create namespace newnamespace
```

Cek apakah namespace telah dibuat

```{.bash .copy}
kubectl get namespace
```

```
NAME                 STATUS   AGE
default              Active   2d5h
kube-node-lease      Active   2d5h
kube-public          Active   2d5h
kube-system          Active   2d5h
local-path-storage   Active   2d5h
newnamespace         Active   2s
```

#### Menggunakan Namespace

Saat membuat resource Kubernetes, kita dapat menentukan Namespace di mana resource tersebut akan dibuat. Sebagai contoh, mari kita buat Pod sederhana di Namespace `newnamespace`.

Buat file dengan nama `my-pod.yaml`:

```{.yaml .copy}
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: newnamespace
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

Simpan file tersebut, lalu jalankan perintah berikut untuk membuat Pod:

```{.bash .copy}
kubectl create -f my-pod.yaml
```

Untuk melihat Pod yang ada di Namespace `newnamespace`, jalankan:

```{.bash .copy}
kubectl get pods --namespace newnamespace
```

Kita juga dapat mengatur Namespace default untuk semua perintah kubectl:

```{.bash .copy}
kubectl config set-context --current --namespace=newnamespace
```

Sekarang, kita cukup menjalankan `kubectl get pods` untuk melihat Pod di Namespace `newnamespace`.

#### Menghapus Namespace

Untuk menghapus Namespace, cukup jalankan perintah berikut:

```{.bash .copy}
kubectl delete namespace newnamespace
```

**Perhatian**: Menghapus Namespace akan menghapus semua resource di dalamnya.

#### Resource Tanpa Namespace

Tidak semua resource dalam Kubernetes terikat pada namespace. Beberapa sumber daya bersifat cluster-wide dan tidak terkait dengan namespace tertentu. Contohnya termasuk `CustomResourceDefinitions (CRDs)`, `Nodes`, `PersistentVolumes`, dan `ClusterRoles`. Resource ini berlaku di seluruh kluster dan tidak dibatasi oleh namespace.
