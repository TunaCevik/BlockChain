Solidity'de `abstract contract`, `library`, ve `interface` farklı amaçlarla kullanılan yapılar olup, her birinin kendine özgü kullanım durumları vardır. Bu yapılar, yazılım geliştiricilere farklı ihtiyaçlara uygun araçlar sunar. İşte her birinin özellikleri ve aralarındaki farklar:

### 1. Abstract Contract

**Abstract Contract**, temel işlevselliği tanımlayan ancak eksik uygulamaları olan bir sözleşmedir. `abstract` olarak işaretlenmiş bir sözleşme, kendi başına dağıtılamaz ve en az bir soyut (abstract) fonksiyon içerir. Soyut fonksiyonlar sadece fonksiyon imzasını tanımlar, ancak fonksiyon gövdesi içermezler. Bu fonksiyonlar, türetilmiş sözleşmelerde uygulanmalıdır.

**Özellikler:**

- Soyut fonksiyonlar tanımlanabilir.
- Durum değişkenleri ve tam uygulanmış fonksiyonlar içerebilir.
- Birden fazla abstract contract'tan türetilebilir.

**Örnek:**

```solidity
abstract contract Animal {
    string public species;

    constructor(string memory _species) {
        species = _species;
    }

    function makeSound() public virtual returns (string memory);
}

contract Dog is Animal {
    constructor() Animal("Dog") {}

    function makeSound() public pure override returns (string memory) {
        return "Bark";
    }
}
```

Bu örnekte, `Animal` adlı soyut sözleşme, `makeSound` adlı bir soyut fonksiyon tanımlar. `Dog` sözleşmesi ise `Animal`'den türemiş olup, `makeSound` fonksiyonunu uygulamaktadır.

### 2. Library

**Library**, yardımcı fonksiyonlar içeren, durum değişkenleri olmayan ve doğrudan diğer sözleşmelerin durumunu değiştiremeyen bir yapıdır. Genellikle hesaplamalar ve yardımcı işlevler için kullanılır. `library`'ler kendi başlarına durum saklamazlar.

**Özellikler:**

- Durum değişkenleri tanımlayamaz.
- Genellikle saf (pure) veya görüntüleme (view) fonksiyonları içerir.
- Kod tekrarını önlemek için kullanılır.

**Örnek:**

```solidity
library Math {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }
}

contract Calculator {
    using Math for uint256;

    function calculateSum(uint256 a, uint256 b) public pure returns (uint256) {
        return a.add(b);
    }
}
```

Bu örnekte, `Math` adlı bir kütüphane, `add` fonksiyonunu sağlar ve `Calculator` sözleşmesi bu kütüphaneyi kullanır.

### 3. Interface

**Interface**, bir sözleşmenin uygulaması gereken fonksiyonların imzalarını tanımlar. Fonksiyon gövdeleri içermez ve durum değişkenleri tanımlayamaz. `interface`, farklı sözleşmelerin nasıl iletişim kuracağını belirlemek için bir standart oluşturur.

**Özellikler:**

- Fonksiyonların yalnızca imzalarını içerir, gövde içermez.
- Durum değişkenleri içermez.
- Sözleşmelerin uyulması gereken bir protokol tanımlar.

**Örnek:**

```solidity
interface IVehicle {
    function start() external;
    function stop() external;
}

contract Car is IVehicle {
    function start() external override {
        // Car starting logic
    }

    function stop() external override {
        // Car stopping logic
    }
}
```

Bu örnekte, `IVehicle` arayüzü, `start` ve `stop` fonksiyonlarını tanımlar. `Car` sözleşmesi, `IVehicle` arayüzünü uygular ve bu fonksiyonların gövdelerini sağlar.

### Karşılaştırma ve Kullanım Durumları

- **Abstract Contract**: Kısmen uygulama içeren ve türetilmiş sözleşmelerde tamamlanması gereken işlevselliği tanımlamak için kullanılır. Durum değişkenleri ve tam fonksiyonlar içerebilir.

- **Library**: Yardımcı işlevler sağlamak ve kod tekrarını önlemek için kullanılır. Durum değişkenleri tanımlayamaz ve doğrudan diğer sözleşmelerin durumunu değiştiremez. Hesaplamalar ve dönüşümler gibi işlemler için idealdir.

- **Interface**: Sözleşmeler arasında bir protokol tanımlamak için kullanılır. Herhangi bir uygulama içermez ve sadece fonksiyon imzalarını tanımlar. Özellikle farklı sözleşmelerin birlikte çalışabilmesi için bir standart oluşturmak amacıyla kullanılır.

Bu yapılar, Solidity'de farklı yazılım ihtiyaçlarını karşılamak için kullanılır ve doğru yapı, geliştiricinin ihtiyaçlarına göre seçilmelidir.
