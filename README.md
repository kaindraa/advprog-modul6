# Module 6: Concurrency

### Kaindra Rizq Sachio
### 2306274964   
### CSCM602223 - Pemrograman Lanjut

---

## Commit 1

`handle_connection(mut stream: TcpStream)`

Method ini memproses request dari klien yang terhubung melalui TCP

TCP (Transmission Control Protocol) adalah protokol komuniksi yang digunakan untuk mengirimkan data antar perangkat dalam jaringan.

Method ini menerima objek dengan tipe data `TcpStream`, yakni tipe data yang merepresentasikan satu konteksi TCP. Selain itu, objek yang diterima juga bersifat mutable

Penjelasan lines:

- `let buf_reader = BufReader::new(&mut stream);`: Kode ini men-wrap `stream` dalam buffered reader agar proses read lebih efisien
- ` let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();`: Kode ini mengambil request header
- `.lines()`: Membaca  `stream` line by line
- `.map(|result| result.unwrap())`: Mengambil nilai string dari setiap `Result<String, io::Error>`. Panic jika terjadi error
- `.take_while(|line| !line.is_empty())`: Read sampai ketemu line kosong (menandakan akhir HTTP)
- `collect` Mengubah lines menjadi `Vec<String>`
- `println!("Request: {:#?}", http_request);`: Mencetak HTTP request yang sudah diproses

`main()`

Method ini memulai server TCP yang mendengarkan koneksi pada port 7878, lalu memproses setiap koneksi masuk menggunakan `handle_connection`

Penjelasan lines:
- `let listener = TcpListener::bind("127.0.0.1:7878").unwrap();`: Membuat TCP Listener di alamat 127.0.0.1:7878 (localhost, port 7878).. Jika gagal (misalnya, port 7878 sudah digunakan oleh program lain), program akan panic dan berhenti (dari `.unwrap()`).

- `for stream in listener.incoming() {
    let stream = stream.unwrap();
    handle_connection(stream);
}`: Loop untuk menerima koneksi dari klien dan memprosesnya.

- `listener.incoming()`: Mendapatkan iterator untuk setiap koneksi yang masuk.

- `stream.unwrap()`: Mengambil TcpStream dari Result<TcpStream, Error>, panic jika gagal.

- `handle_connection(stream);`: Memproses koneksi klien dengan fungsi handle_connection().

    

 