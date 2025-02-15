# WhatsApp Clone SwiftUI Firebase iOS 18 2025  

| Giriş İşlemi | Mesaj Atmak | Mesaj Almak |
|-------------|------------|------------|
| <img src="https://github.com/user-attachments/assets/176ef78f-0f25-4980-87e9-a7122808f1ef" width="300"/> | <img src="https://github.com/user-attachments/assets/b2288db6-8835-4dd4-b513-ae9ed86f30e4" width="300"/> | <img src="https://github.com/user-attachments/assets/ded9de59-3066-43a1-8f8d-fae9bc0b50d2" width="300"/> |



🔥 Kullanılan Teknolojiler

SwiftUI: Modern ve deklaratif bir kullanıcı arayüzü oluşturmak için kullanıldı.
Firebase Authentication: Kullanıcıların e-posta/parola veya diğer kimlik doğrulama yöntemleriyle giriş yapmasını sağlar.
Cloud Firestore: Gerçek zamanlı mesajlaşma için kullanılan NoSQL veritabanı. Sohbetler ve kullanıcı bilgileri burada saklanır.
Firebase Storage: Kullanıcıların profil fotoğrafları ve medya paylaşımlarını saklamak için kullanıldı.
Combine Framework: State yönetimi ve veri akışlarını yönetmek için kullanıldı.
Bu projede kullanıcı kimlik doğrulama sürecinden, mesajların Firestore üzerinden senkronize edilmesine kadar birçok temel özellik Firebase servisleri ile sağlanmaktadır.

## Özellikler
- **Kullanıcı Kimlik Doğrulaması**: Firebase Authentication, kullanıcı girişi ve kaydı için kullanılır.
- Gerçek Zamanlı Mesajlaşma**: Mesajlar Firestore kullanılarak saklanır ve alınır.
- Durum Yönetimi**: Proje, durum yönetimi için `ObservableObject` ve `Combine` kullanır.
- Modern kullanıcı arayüzü**: Temiz ve duyarlı bir tasarım için SwiftUI ile oluşturulmuştur.

## Project Structure
```
WhatsAppCloneSwiftUI/
│-- WhatsAppCloneSwiftUIApp.swift
│-- ContentView.swift
│-- AuthViewModel.swift
│-- UserModel.swift
│-- UserStore.swift
│-- ChatView.swift
│-- ChatModel.swift
│-- ChatStore.swift
│-- ChatRow.swift
│-- Preview Content/
│-- Assets/
│-- GoogleService-Info.plist
```

## Example Code
### ChatStore (State Management)
```swift
import Foundation
import SwiftUI
import FirebaseAuth
import FirebaseFirestore
import Combine

class ChatStore: ObservableObject {
    let db = Firestore.firestore()
    var chatArray: [ChatModel] = []
    var objectWillChange = PassthroughSubject<Array<Any>, Never>()
    
    init() {
        db.collection("Chats").whereField("chatUserFrom", isEqualTo: Auth.auth().currentUser?.uid).addSnapshotListener { snapshot, error in
            if error == nil {
                self.chatArray.removeAll(keepingCapacity: false)
                for document in snapshot!.documents {
                    let chatUidFromFirebase = document.documentID
                    if let chatMessage = document.get("message") as? String,
                       let messageFrom = document.get("chatUserFrom") as? String,
                       let messageTo = document.get("chatUserTo") as? String,
                       let dateString = document.get("date") as? String {
                        let dateFormatter = DateFormatter()
                        dateFormatter.dateFormat = "yyyy_MM_dd_hh_mm_ss"
                        if let dateFromFB = dateFormatter.date(from: dateString) {
                            let currentIndex = self.chatArray.last?.id ?? -1
                            let createdChat = ChatModel(id: currentIndex + 1, message: chatMessage, uidFromFirebase: chatUidFromFirebase, messageFrom: messageFrom, messageTo: messageTo, messageDate: dateFromFB, messageFromMe: true)
                            self.chatArray.append(createdChat)
                        }
                    }
                }
                self.objectWillChange.send(self.chatArray)
            }
        }
    }
}
```

### ChatModel
```swift
import Foundation
import SwiftUI

struct ChatModel: Identifiable {
    var id: Int
    var message: String
    var uidFromFirebase: String
    var messageFrom: String
    var messageTo: String
    var messageDate: Date
    var messageFromMe: Bool
}
```

## Katkı
Projeye katkıda bulunmak istiyorsanız, lütfen bir **fork** oluşturun ve bir **pull request** gönderin.

## Özel Depo
Proje, API anahtarlarının en başından beri taahhüt edilmesi nedeniyle şu anda özeldir. Katkıda bulunmakla ilgileniyorsanız, lütfen erişim için bir e-posta isteği gönderin.

## Lisans
Bu proje MIT Lisansı altında lisanslanmıştır.

