# Reflection on Hello Minikube
1.  Compare the application logs before and after you exposed it as a service. Try to open the app several times while the proxy into the service is running. What do you see in the logs? Does the number of logs increase each time you open the app?
- Sebelum aplikasi diekspos jadi sebuah service, log cuma menunjukkan kalo server HTTP dan UDP udah nyala di port 8080 dan 8081. Setelah aplikasi diekspos jadi layanan, kita bisa nyambung dan komunikasi sama server dari luar klaster Kubernetes. Kalo kita buka aplikasi pake proxy ke service yang lagi jalan beberapa kali, lognya bakal makin besar. Hal ini karena server punya fitur logging yang catet setiap permintaan HTTP yang masuk sama waktu kapan masuknya. Tiap kali kita buka aplikasi, permintaan HTTP dikirim ke server, yang bikin log catat pesan lagi.

2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
- Opsi `-n` pada perintah `kubectl get` digunakan untuk menunjukkan namespace yang ingin ditampilkan. Namespace adalah cara untuk mengelompokkan objek Kubernetes ke dalam unit yang terisolasi. Namespace awal adalah `default`, tetapi namespace bisa dibuat khusus sesuai kebutuhan.
- Opsi `-n` dengan nilai yang diatur ke `kube-system` mengambil semua pod dan layanan dalam namespace `kube-system`. Sebelumnya, layanan dan pod berada di namespace yang berbeda, `default`. itulah alasan mengapa layanan/pod yang telah dibuat sebelumnya tidak terlihat dalam output.

# Reflection on Rolling Update & Kubernetes Manifest File
1. What is the difference between Rolling Update and Recreate deployment strategy?
- Rolling Update: Strategi ini memungkinkan kita untuk memperbarui kumpulan pod tanpa adanya downtime. Kumpulan pod yang menjalankan versi aplikasi lama digantikan satu per satu dengan versi baru. Dengan demikian, aplikasi tetap tersedia selama proses pembaruan berlangsung.
- Recreate: Strategi ini menghapus semua pod yang ada dan kemudian melakukan redeploy pod dengan versi baru. Meskipun ini menyebabkan adanya downtime pada aplikasi, strategi ini sederhana dan cocok untuk aplikasi yang toleran terhadap jeda sejenak.

2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document
your attempt.
- Mengubah strategi dalam file deployment.yaml menjadi "recreate", lalu terapkan perubahan tersebut pada file deployment.yaml.

3. Prepare different manifest files for executing Recreate deployment strategy.
-

4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.
- Manifest memungkinkan kita mendefinisikan keadaan yang diinginkan untuk objek Kubernetes secara deklaratif. Kita hanya perlu menyatakan apa yang kita inginkan, dan Kubernetes akan menangani detail implementasinya.
- Manifest memungkinkan kita mengelola objek Kubernetes sebagai kode. Ini memudahkan pelacakan perubahan dan kolaborasi dalam mengelola objek.
- Manifest memungkinkan otomasi dan alur kerja CI/CD dengan mudah.
