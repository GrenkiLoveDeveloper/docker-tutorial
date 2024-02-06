# Установка Docker и WSL на Windows

## Установка WSL и Ubuntu

1. Откройте CMD/Powershell от имени Администратора и введите следующую команду для установки WSL:

```wsl --install

```

2. После перезагрузки установите дистрибутив Ubuntu и настройте его на WSL2:

```
   wsl --install -d Ubuntu
   wsl --set-version ubuntu 2
```

3. Откройте cmd и введите название выбранного ранее дистрибутива. Вы перейдете в bash linux, где требуется создать пользователя.

## Установка Docker

1. Перейдите на [официальный сайт Docker](https://www.docker.com/products/docker-desktop) и скачайте Docker Desktop для Windows.
2. Запустите установщик Docker Desktop.
3. Следуйте инструкциям на экране для завершения установки. Docker Desktop будет запущен после установки. Проверьте, что он запущен, проверив значок Docker в системном трее.
4. Добавить пользователя в группу docker users командой в PowerShell от имени администратора:

```
net localgroup docker-users "login pc" /ADD
```

5. Откройте командную строку или PowerShell и введите следующую команду, чтобы проверить, что Docker установлен правильно:

```bash
docker run hello-world
```

# Регулировка использования ресурсов докером

Создайте файл .wslconfig в папке пользователя (C:\Users\ [Имя_Пользователя]\ ) и укажите ограничения по использованию ОЗУ, CPU и т.д.
[Подробнее](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig)

```[wsl2]
    memory=4GB
```

# Размещение проекта

Для корректной работы Docker и WSL файлы проектов должны находиться в файловой системе linux. Чтобы попасть в файловую систему WSL, можно открыть её в проводнике. Для контроля над файлами установите git и настройте SSH-ключ: [Настройка SSH-ключа](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

```
sudo apt-get update
sudo apt-get install git
```

После этого можно клонировать проекты в файловую систему WSL
Для корректной работы IDE в WSL рекомендуется дать права на запись всем пользователям(На папку проекта)

```
sudo chmod -R 777 /folder
```

### Для управления версиями Node рекомендуется установить [NVM](https://github.com/nvm-sh/nvm#installing-and-updating).

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

# IDE

### VS Code

Для запуска VS Code в WSL необходимо установить расширение WSL и запустить из консоли

```code .

```

# Аллиасы

### Git Bash

В папке пользователя (C:\Users\ [Имя_Пользователя]\ ) можно создать файл .bash_profile и вставить алиасы... например:

```
    alias dclogs='winpty docker-compose logs -f'
```

или указать путь до файла с алиасами

```
source путь/до/файла
```

### для PowerShell

1. Разрешите политику функций (в PS от имени Администратора):

```
Set-ExecutionPolicy RemoteSigned
```

2. Создайте файл для хранения алиасов:

```
New-Item -Path $profile -ItemType file -force
```

3. Откройте файл и добавьте в него алиасы:

```
notepad $profile
```

Пример алиаса:

```
function Docker-Exec {
    cd .docker/docker-compose
    docker compose exec app php @Args
    cd ..
    cd ..
}
Set-Alias dphp Docker-Exec
```

# Решение возможных проблем

1. Перед установкой WSL удостоверьтесь, что в BIOS включена виртуализация(Настройка зависит от BIOS,по этому гугл в помощь)

2. Затем необходимо установить правило + пакет обновления с [официального сайта Microsoft](https://learn.microsoft.com/ru-ru/windows/wsl/install-manual#step-3---enable-virtual-machine-feature)

3. При разрушительном сбое при попытке установки Ubuntu рекомендуется обновить Microsoft Store

4. v.4.21.0
