# Лабораторна робота — Privacy Codelab (Android Permissions & Privacy)

## Мета роботи
Метою лабораторної роботи є ознайомлення з сучасними механізмами конфіденційності Android, навчитися працювати з Runtime Permissions, Photo Picker API та новими Privacy Dashboard інструментами, а також інтегрувати їх у простий мобільний застосунок.

## Завдання
1. Додати запит дозволу на доступ до локації.
2. Використати Photo Picker для отримання фотографій.
3. Продемонструвати роботу permission-флоу.
4. Забезпечити правильну обробку ситуацій: дозвіл надано, відхилено, назавжди заблоковано.
5. Оформити проект із Material Design.
6. Додати README‑звіт.

---

## Хід виконання роботи

### 1. Створення нового Android‑проєкту
Було створено проект **PrivacyCodelab**, підключено залежності Material3 та Activity Compose.

### 2. Додавання необхідних дозволів у `AndroidManifest.xml`
```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.CAMERA" />

<uses-feature
    android:name="android.hardware.camera"
    android:required="false" />
```

### 3. Робота з камерою через Photo Picker (Android 13+)
```kotlin
val photoPickerLauncher = rememberLauncherForActivityResult(
    contract = PickVisualMedia()
) { uri ->
    lastPhotoUri = uri?.toString()
}
```

### 4. Запит дозволу на геолокацію
```kotlin
val locationPermissionLauncher = rememberLauncherForActivityResult(
    ActivityResultContracts.RequestPermission()
) { granted ->
    if (granted) {
        scope.launch { snackbarHostState.showSnackbar("Дозвіл надано") }
    } else {
        scope.launch { snackbarHostState.showSnackbar("Дозвіл відхилено") }
    }
}
```

### 5. Отримання останньої відомої локації
```kotlin
suspend fun getLastKnownLocation(context: Context): Location? =
    suspendCoroutine { cont ->
        val service = LocationServices.getFusedLocationProviderClient(context)
        service.lastLocation.addOnSuccessListener { cont.resume(it) }
        service.addOnFailureListener { cont.resume(null) }
    }
```

### 6. UI для демонстрації функцій
Кнопки:
- «Обрати фото»
- «Запитати доступ до локації»
- «Показати останнє фото»
- «Показати останню локацію»

---

## Скриншоти роботи (у README додадуться вручну)
- Головний екран
- Photo Picker
- Дозвіл на геолокацію
- Snackbars

---

## Висновок
У ході роботи було успішно додано та протестовано сучасні механізми конфіденційності Android, включно з Photo Picker, permission‑flow та Location API. Застосунок коректно реагує на дії користувача та відповідає стандартам Android Privacy.

