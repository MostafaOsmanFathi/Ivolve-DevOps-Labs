```
mostafaosman@zenbook-ux3402za:~/WORK/IVolve/Ivolve-DevOps-Labs/Docker/lab-2$ docker image ls | grep calc
calc-app:java      41fde08c7d80   586MB   151MB   U
calc-app:latest    61057a134ffd   857MB   266MB   U
```

`calc-app:java` is smaller than `calc-app:latest` because `calc-app:java` only contains the `.jar` file and nothing more, while the Maven image contains the application source code, compiled `.class` files, and application dependencies, which take up more space.