# WeakMap
---
위크맵(WeakMap)은 맵(Map)과 같은 자료구조로 key, value의 형태로 저장한다. 다만 맵 과의 차이점은 키에 올 수 있는 데이터는 참조형 데이터인 객체와 심벌(Symbol)만 가능하다. 따라서 일반 원시형 데이터는 키로 올 수 없다.
또 위크맵이 보유한 키들은 맵 처럼 열거할 수 있는 메서드가 존재하지 않는다.

위크맵의 가장 큰 특징은 데이터를 약한 참조를 한다. 예를 들어 맵의 경우 저장한 키 객체에 대해 참조가 끊기더라도 해당 맵이 참조를 하고 있어 가비지 컬렉터가 이를 수거하지 않는다. 
```js
let me = { name: 'Ham' };

const map = new Map();
map.set(me, '');

// 객체에 대한 참조를 끊더라도 map이 참조를 하고 있기 때문에 수거되지 않는다.
me = null;

```

하지만 위크맵의 경우 약한 참조로 가비지 컬렉터에 참조 관계를 알리지 않아 해당 객체의 참조가 끊기게 되면 위크맵의 참조는 관여되지 않아 가비지 컬렉터에 의해 수거된다.
```js
let me = { name: 'Ham' };

const weakMap = new WeakMap();
weakMap.set(me, '');

// me 에 대한 참조가 끊기고 weakMap의 참조는 있지만 약한 참조로 가비지 컬렉터는 알지 못하기 때문에 메모리를 수거해 간다.
me = null;
```

## 사용예시
약한 참조의 특성으로 서드파티 라이브러리에서 주로 사용된다. 서드파티에서 참조를 하더라도 해당 객체가 더 이상 필요하지 않게 됐을때 수거될 수 있다.

캐시에서도 사용될 수 있다. 캐시는 일시적으로 사용되다가 참조가 끊기게 되면 자연스레 메모리를 수거할 수 있기 때문이다.

프라이빗 필드에서 사용된다. 예를 들어 인스턴스 에서 숨기고자 하는 프로퍼티를 값으로 하여 위크맵에 참조시켜 사용할 수 있다.
만약 해당 프로퍼티를 참조하려면 위크맵, 인스턴스 둘 다 알아야 하기 때문에 숨길 수 있게 된다.
```js
const passwordMap = new WeakMap();

class User {
	constructor () {
		this.id = 'ham';
		passwordMap.set(this, '...');
	}
}
```
https://ui.toast.com/posts/ko_20210901


