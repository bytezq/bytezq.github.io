https://cn.electron-vite.org/

typescript问号的使用
```typescript
// 这里表示参数 field 是一个可选参数
function getUser(user: string, field?: string) {
}

// 表示这个name属性有可能不存在
class A {
  name?: string
}

interface B {
  name?: string
}


`person?.name?.firstName` 可以理解成 `person && person.name && person.firstName`

```

感叹号
```typescript
// ! 就是将之后的结果取反，比如：
const a = !isNumber(input);


// 因为接口B里面name被定义为可空的值，但是实际情况是不为空的，那么我们就可以
// 通过在class里面使用！重新强调了name这个不为空值
class A implemented B {
  name!: string
}

interface B {
  name?: string
}
```

[tsconfig配置说明](https://www.tslang.cn/docs/handbook/tsconfig-json.html)
