# Создадим скрипт myapp.py, который будет запускаться в контейнере и каждую секунду записывать в файл timer.txt продолжительность своей работы.
    root@194-58-108-112:~# mkdir -p seminar4/test
    root@194-58-108-112:~# cat >> seminar4/myapp.py
    import pathlib,time
    timer=0
    while True:
        pathlib.Path("/test/timer.txt").write_text(f"myapp is working {timer} seconds")
        time.sleep(1)
        timer+=1
# Создадим Dockerfile и образ на его основе.
    root@194-58-108-112:~# cat >> seminar4/Dockerfile
    FROM ubuntu:22.04
    WORKDIR /
    COPY myapp.py .
    RUN apt-get update && apt-get install -y python3
    ENTRYPOINT ["python3", "myapp.py"]

    root@194-58-108-112:~# docker build -t myapptest seminar4
    [+] Building 16.0s (8/8) FINISHED
    => [internal] load build definition from Dockerfile                                                       0.0s
    => => transferring dockerfile: 167B                                                                       0.0s
    => [internal] load .dockerignore                                                                          0.0s
    => => transferring context: 2B                                                                            0.0s
    => [internal] load metadata for docker.io/library/ubuntu:22.04                                            0.0s
    => [internal] load build context                                                                          0.0s
    => => transferring context: 192B                                                                          0.0s
    => CACHED [1/4] FROM docker.io/library/ubuntu:22.04                                                       0.0s
    => [2/4] COPY myapp.py .                                                                                  0.0s
    => [3/4] RUN apt-get update && apt-get install -y python3                                                14.8s
    => exporting to image                                                                                     1.0s
    => => exporting layers                                                                                    1.0s
    => => writing image sha256:18ad5f518eb4ebd0e3acf290750983f9c77a6fbcbc9f3fec58a44bd82e918f8b               0.0s
    => => naming to docker.io/library/myapptest                                                               0.0s
# Запустим контейнер из нашего образа и проверим работу myapp.py, прочитав несколько раз данные из файла timer.txt.
    root@194-58-108-112:~# docker run -d -v /root/seminar4/test:/test myapptest
    b34b1d1c1c078e731e9b1e03664c548519d4ba9a139f53de4f8f2c21d4d13bbf
    root@194-58-108-112:~# cat seminar4/test/timer.txt
    myapp is working 66 seconds
    root@194-58-108-112:~# cat seminar4/test/timer.txt
    myapp is working 89 seconds
    root@194-58-108-112:~# cat seminar4/test/timer.txt
    myapp is working 114 seconds
    root@194-58-108-112:~# cat seminar4/test/timer.txt
    myapp is working 137 seconds
    root@194-58-108-112:~# cat seminar4/test/timer.txt
    myapp is working 158 seconds