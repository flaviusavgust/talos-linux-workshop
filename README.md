# Talos Linux Workshop for DevOps Days 2024

Добро пожаловать на практическое занятие по Talos Linux! Здесь вы найдете все необходимые инструкции для работы. 

## Список машин Talos Linux с публичными адресами

[Table with White IP Talos Linux VMs](https://docs.google.com/spreadsheets/d/1Ka_Ru00UYV6pgMDJsYa5aTel1Cu8JeiHs60cwn5-NU4/edit?usp=sharing)

## Установка `talosctl`

### macOS
```bash
brew install siderolabs/tap/talosctl
```

### Linux или WSL on Windows
```bash
curl -sL https://talos.dev/install | sh
```

### Windows
1. Скачайте [talosctl-windows-amd64.exe](https://github.com/siderolabs/talos/releases/download/v1.7.4/talosctl-windows-amd64.exe).
2. Переименуйте файл в `talosctl.exe`.
3. Переместите его в папку `C:\bin`. Если папки `C:\bin` еще не существует, создайте ее.
4. Добавьте `C:\bin` в системную переменную `PATH` с помощью команды (запустите командную строку от имени администратора):
   ```cmd
   setx PATH "%PATH%;C:\bin"
   ```

# Начало работы с Talos Linux

## Проверка состояния машин

Сначала проверьте доступность API Talos с помощью `telnet`:
```bash
telnet <IP> 50000
```

Затем убедитесь, что API Talos отвечает:
```bash
talosctl -n <IP> disks --insecure
```

## Генерация секретов и конфигураций

Сгенерируйте секреты для Talos:
```bash
talosctl gen secrets -o secrets.yaml
```

Создайте конфигурации для кластера:
```bash
talosctl gen config <cluster-name> https://<cluster-endpoint>:<port> --with-secrets secrets.yaml
```

## Работа с `talosconfig`

Назначьте endpoint в `talosconfig` и сохраните его:
```bash
talosctl --talosconfig=talosconfig config endpoint <IP>
```

Переместите `talosconfig` в домашнюю директорию:
```bash
mkdir ~/.talos
cp talosconfig ~/.talos/config
```

## Применение конфигураций

Примените конфигурации для `control-plane` и `worker`:
```bash
talosctl apply-config -f controlplane.yaml -n <IP> --insecure
talosctl apply-config -f worker.yaml -n <IP> --insecure
```

## Проверка кластера

Проверьте, что кластер Talos собран:
```bash
talosctl -n <IP> get members
```

Просмотрите логи и мониторинг:
```bash
talosctl -n <IP> dashboard
```

## Проверка состояния сервисов Talos

Для проверки состояния сервисов используйте команды:
```bash
talosctl -n <IP> service
talosctl -n <IP> logs -f <service_name>
```

## Bootstrap Kubernetes в Talos

Запустите bootstrap Kubernetes:
```bash
talosctl -n <IP> bootstrap
```

Получите `kubeconfig`:
```bash
talosctl -n <IP> kubeconfig .
```

## Проверка кластера Kubernetes

Проверьте состояние нод и подов:
```bash
kubectl --kubeconfig=kubeconfig get nodes
kubectl --kubeconfig=kubeconfig get pods -A
```
