ОБЩИЕ ОТЗЫВЫ (Firebase Firestore, бесплатно) — один раз, ~5 минут

1. Зайдите на https://console.firebase.google.com -> "Add project" (название любое).
2. Build -> Firestore Database -> Create database -> Production mode -> регион на выбор.
3. Вкладка Rules, вставьте и нажмите Publish:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /reviews/{id} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasOnly(['name','text','rating','ts'])
                    && request.resource.data.name is string && request.resource.data.name.size() <= 40
                    && request.resource.data.text is string && request.resource.data.text.size() <= 400
                    && request.resource.data.rating is int
                    && request.resource.data.rating >= 1 && request.resource.data.rating <= 5;
      allow update, delete: if false;
    }
  }
}

4. Project settings (шестерёнка) -> General -> Your apps -> значок </> (Web) -> зарегистрируйте приложение.
   Скопируйте apiKey и projectId.
5. В index.html найдите  const FIREBASE = { projectId: "", apiKey: "" };  и вставьте значения.
6. Загрузите index.html на GitHub. Готово: отзывы теперь видят все.

Удалять неприличные отзывы можно в консоли Firebase -> Firestore -> reviews.
