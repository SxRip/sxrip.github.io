---
title: "Hello World"
description: Короткое описание — уходит в og:description и в подзаголовок поста.
date: 2026-07-21 12:00:00 +0200
categories: [reverse engineering, devirtualization] # [родитель, ребёнок]
tags: [asm, deobfuscation] # плоские, строчными; из них строится Trending Tags
media_subpath: /assets/img/posts/hello-world/ # префикс для всех картинок ниже
image:
  path: cover.png # обложка карточки на главной + og:image, ~1200x630
  alt: cover
pin: true # булавка на карточке
toc: true
comments: true
mermaid: true
math: false
---

Первые ~200 символов текста попадают в превью карточки на главной, так что
не начинай пост с воды.

## Код

```c
NTSTATUS DriverEntry(_In_ PDRIVER_OBJECT DriverObject,
                     _In_ PUNICODE_STRING RegistryPath)
{
    UNREFERENCED_PARAMETER(RegistryPath);
    DriverObject->DriverUnload = DriverUnload;
    return STATUS_SUCCESS;
}
```
{: file="src/driver.c" }

Инлайн-путь оформляется так: `C:\Windows\System32\ntoskrnl.exe`{: .filepath }

## Плашки

> Полезно знать.
{: .prompt-tip }

> Осторожно, тут ловушка.
{: .prompt-warning }

## Картинка

Файл лежит в `assets/img/posts/hello-world/`, в тексте пишется только имя,
потому что задан `media_subpath`.

<!-- ![дизасм](disasm.png){: width="800" height="400" }
_Подпись под картинкой_ -->

## Mermaid

```mermaid
graph LR
  A[usermode] -->|NtDeviceIoControlFile| B[IRP_MJ_DEVICE_CONTROL]
  B --> C[dispatch routine]
```

## Сноски и ссылки

Сноска[^1] и ссылка на [документацию темы](https://chirpy.cotes.page/).

[^1]: Текст сноски.
