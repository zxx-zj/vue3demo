<template>
    <div>
    </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref, watch, watchEffect } from "vue";
import type { Ref } from "vue";
import { onMounted, onUpdated } from "vue";
import { useRouter } from "vue-router";

const formData = ref('')
const input2 = ref('')
const year: Ref<string | number> = ref('2000')

const day = ref('2000')
//ts与js区别主要在于ts需要定义类型
let abc: string;

const name = reactive({
    firstName: 'first',
    lastName: 'last',
})

//ref解包
const count = ref(1)
const obj = reactive({ count })
console.log(obj.count === count.value, 'ff');//true
// 会更新obj.count
count.value++;
console.log(count.value) // 2
console.log(obj.count) // 2
// 也会更新 `count` ref
obj.count++;
console.log(obj.count) // 3
console.log(count.value) // 3

//当访问到某个响应式数组或Map这样的原生集合类型中的ref元素时，不会执行ref的解包
const books = reactive([ref('vue3 study')])
console.log(books[0].value);
const map = reactive(new Map([['count', ref(0)]]))
console.log(map.get('count')?.value);

//将一个ref赋值给为一个reactive属性时，该ref会被自动解包
const countR = ref(1)
const objR = reactive({})
objR.countR = countR
console.log(objR.countR);//1
console.log(objR.countR === countR.value);//true


//computed:计算属性
//期望接收到一个getter函数，返回值为一个计算属性ref
const author = reactive({
    name: 'John Doe',
    books: [
        'vue2 - aaa',
        'vue3 - bbb',
        'vue4 - ccc'
    ]
})
const publishedBooksMessage = computed(()=>{
    return author.books.length > 0 ? 'yes' : 'no'
})
console.log(publishedBooksMessage.value);//yes
//计算属性缓存vs方法
//若我们将同样的函数定义为一个方法而不是计算属性，
//两种方式在结果上确实是完全相同的，
//然而，不同之处在于计算属性值会基于其响应式依赖被缓存


//Class 与 Style 绑定 
//class 和 style 都是 attribute，
//我们可以和其他attribute一样使用 v-bind将它们和动态的字符串绑定
//组件上和dom上均可以

//条件渲染
//v-if
//1、v-if 是“真实的”按条件渲染
//因为它确保了在切换时，条件区块内的事件监听器和子组件都会被销毁与重建。
//2、v-if是惰性的
//如果在初次渲染时条件值为 false，则不会做任何事。条件区块只有当条件首次变为 true 时才被渲染
//3、v-show：元素初始条件如何，始终都会被渲染，只有css的display属性会被切换。
//总的来说v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
//因此，如果需要频繁切换，则使用 v-show 较好；
//如果在运行时绑定条件很少改变，则 v-if 会更合适。
//当 v-if 和 v-for 同时存在于一个元素上的时候，v-if 会首先被执行


//列表渲染v-for
//需要使用item in items形式的特殊语法，items的源数据数组，item是迭代的别名
//类似于
const parentMessage = 'Parent'
const items = [{ message: 'Foo' }, { message: 'Bar' }]
items.forEach((item: { message: any; }, index: any) => {
    console.log(parentMessage, item.message, index); 
});
//v-for对象
const myObj = reactive({
    title: 'How to do list in vue',
    author: 'Jane Doe',
    publishedAt: '2023-04-19'
})
// <ul>
//   <li v-for="value in myObject">
//     {{ value }}
//   </li>
// </ul>
//v-for通过key管理状态
//Vue 默认按照“就地更新”的策略来更新通过 v-for 渲染的元素列表
//当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序
//而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。
//默认模式是高效的
//但只适用于列表渲染输出的结果不依赖子组件状态或者临时 DOM 状态 (例如表单输入值) 的情况。
//key：number | string | symbol都可以

//事件处理v-on（简写@）：监听Dom事件，并在事件触发时执行对应的JavaScript。
//用法：v-on：click='handler'或者@click='handler'

// 1、内联事件处理器：事件被触发时执行的内联JavaScript语句（与onclick类型）
const count1 = ref(0)
// <button @click="count++">Add 1</button>
// <p>Count is: {{ count }}</p>


// 2、方法事件处理器：一个指向组件上定义的方法的属性名或是路径
const name1 = ref('Vue.js')
function greet(event1: { target: { tagName: any; }; }) {
    alert('hello ${name1.value}')
    if(event1){
        alert(event1.target.tagName)
    }
}
// <button @click="greet">Greet</button>
// 方法事件处理器会自动接收原生DOM事件并触发执行。
// 我们能够通过被触发事件的event.target.tagName访问到该DOM元素。
//时间修饰符
//.stop-<a @click.stop="doThis"></a>
//.prevent-<form @submit.prevent="onSubmit"></form>
//.self-<div @click.self="doThat">...</div>
//.capture-<div @click.capture="doThis">...</div>
//.once-<a @click.once="doThis"></a>
//.passive-<div @scroll.passive="onScroll">...</div>
//等等等


//生命周期
//1、onMounted()：在组件完成初始渲染并创建 DOM 节点后运行代码
//注册一个回调函数，在组件挂载完成后执行
// function onMounted (callback: () => void): void
//通常用于执行需要访问组件所渲染的 DOM 树相关的副作用
//或是在服务端渲染应用中用于确保 DOM 相关代码仅在客户端执行
//在服务器端渲染期间不会被调用

//2、onUpdated():在组件因为响应式状态变更而更新其 DOM 树之后调用
//function onUpdated(callback: () => void): void
//父组件的更新钩子将在其子组件的更新钩子之后调用
//会在组件的任意 DOM 更新后被调用
//更新可能是由不同的状态变更导致的
//需要在某个特定的状态更改后访问更新后的 DOM
//使用 nextTick() 作为替代
//在服务器端渲染期间不会被调用
//不要在 updated 钩子中更改组件的状态，这可能会导致无限的更新循环！

//3、onUnmounted():在组件实例被卸载之后调用
//function onUnmounted(callback: () => void): void
//组件被卸载情况：
//1、其所有子组件都已经被卸载
//2、所有相关的响应式作用 (渲染作用以及 setup() 时创建的计算属性和侦听器) 都已经停止。
//可以在这个钩子中手动清理一些副作用，例如计时器、DOM 事件监听器或者与服务器的连接。
//在服务器端渲染期间不会被调用

//4、onBeforeMount():在组件被挂载之前被调用
//function onBeforeMount(callback: () => void): void
//当这个钩子被调用时，组件已经完成了其响应式状态的设置
//但还没有创建 DOM 节点。它即将首次执行 DOM 渲染过程。
//在服务器端渲染期间不会被调用

//5、onBeforeUpdate(): 在组件即将因为响应式状态变更而更新其 DOM 树之前调用
//function onBeforeUpdate(callback: () => void): void
//这个钩子可以用来在 Vue 更新 DOM 之前访问 DOM 状态。
//这个钩子中更改状态也是安全的。
//在服务器端渲染期间不会被调用

//6、onBeforeUnmount(): 在组件实例被卸载之前调用
//function onBeforeUnmount(callback: () => void): void
//当这个钩子被调用时，组件实例依然还保有全部的功能。
//在服务器端渲染期间不会被调用

//7、onErrorCaptured(): 在捕获了后代组件传递的错误时调用
//错误捕获来源：组件渲染、事件处理器、生命周期钩子、setup()函数、侦听器、自定义指令钩子、过渡钩子

//8、onRenderTracked(): 组件渲染过程中追踪到响应式依赖时调用
//仅在开发模式下可用，且在服务器端渲染期间不会被调用

//9、onRenderTriggered()：响应式依赖的变更触发了组件渲染时调用
// 仅在开发模式下可用，且在服务器端渲染期间不会被调用。

//10、onActivated()：组件实例是 <KeepAlive> 缓存树的一部分，当组件被插入到 DOM 中时调用
//在服务器端渲染期间不会被调用
//function onActivated(callback: () => void): void

//11、onDeactivated()：组件实例是 <KeepAlive> 缓存树的一部分，当组件从 DOM 中被移除时调用
//在服务器端渲染期间不会被调用
//function onDeactivated(callback: () => void): void

//12、onServerPrefetch()：组件实例在服务器上被渲染之前调用
//function onServerPrefetch(callback: () => Promise<any>): void
//如果这个钩子返回了一个 Promise，服务端渲染会在渲染该组件前等待该 Promise 完成。
//这个钩子仅会在服务端渲染中执行，可以用于执行一些仅存在于服务端的数据抓取过程。


//依赖注入
//provide():提供一个值，可以被后代组件注入。
//function provide<T>(key: InjectionKey<T> | string, value: T): void
//provide() 接受两个参数
//第一个参数是要注入的 key，可以是一个字符串或者一个 symbol，第二个参数是要注入的值。
//inject():注入一个由祖先组件或整个应用 (通过 app.provide()) 提供的值。
//第一个参数是注入的 key,第二个参数是可选的，即在没有匹配到 key 时使用的默认值
//二者相同之处：
//当使用 TypeScript 时，key 可以是一个被类型断言为 InjectionKey 的 symbol
//InjectionKey是一个 Vue提供的工具类型
//继承自Symbol，可以用来同步provide()和inject()之间值的类型。
//与注册生命周期钩子的 API 类似，provide() 必须在组件的 setup() 阶段同步调用。




//侦听器（watch）：
//watch 的第一个参数可以是不同形式的“数据源”
//它可以是一个ref(包括计算属性)、一个响应式对象、一个getter函数、或多个数据源组成的数组
// const x = ref(0)
// const y = ref(0)
// // 单个 ref
// watch(x, (newX) => {
//   console.log(`x is ${newX}`)
// })
// // getter 函数
// watch(
//   () => x.value + y.value,
//   (sum) => {
//     console.log(`sum of x + y is: ${sum}`)
//   }
// )
// // 多个来源组成的数组
// watch([x, () => y.value], ([newX, newY]) => {
//   console.log(`x is ${newX} and y is ${newY}`)
// })
//不能直接侦听响应式对象的属性值
// 需要用一个返回该属性的 getter 函数
// const obj = reactive({ count: 0 })

// // 错误，因为 watch() 得到的参数是一个 number
// watch(obj.count, (count) => {
//   console.log(`count is: ${count}`)
// })

// // 提供一个 getter 函数
// watch(
//   () => obj.count,
//   (count) => {
//     console.log(`count is: ${count}`)
//   }
// )

//watchEffect即时回调的侦听器
const todoID = ref(1)
const data = ref(null)

//watch和watchEffect使用例子

//侦听器两次使用todoID，一次是作为源，另一次是在回调中
watch(todoID, async () => {
    const response = await fetch(
        `https//jsonplaceholder.typicode.com/todos/${todoID.value}`
    )
    data.value = await response.json()
}, {immediate:true})

//watchEffect()允许我们自动跟踪回调的响应式依赖
//不需要指定 immediate: true
//执行期间，它会自动追踪 todoId.value 作为依赖（和计算属性类似）
//每当 todoId.value 变化时，回调会再次执行
//不再需要明确传递 todoId 作为源值
watchEffect(async () =>{
    const response = await fetch(
        `https//jsonplaceholder.typicode.com/todos/${todoID.value}`
    )
    data.value = await response.json()
})
//tip：watchEffect仅会在其同步执行期间，才追踪依赖。
//在使用异步回调时，只有在第一个await正常工作前访问到的属性才会被追踪。

//watch  VS  watchEffect
//watch 和 watchEffect 都能响应式地执行有副作用的回调
//主要区别是追踪响应式依赖的方式
//1、watch 只追踪明确侦听的数据源，不会追踪任何在回调中访问到的东西
//仅在数据源确实改变时才会触发回调
//watch 会避免在发生副作用时追踪依赖，因此，我们能更加精确地控制回调函数的触发时机
//2、watchEffect，则会在副作用发生期间追踪依赖
//会在同步执行过程中，自动追踪所有能访问到的响应式属性
//更方便，而且代码往往更简洁，但有时其响应性依赖关系会不那么明确

//回调的触发时机
//更改了响应式状态，它可能会同时触发 Vue 组件更新和侦听器回调
//默认情况下，用户创建的侦听器回调，都会在 Vue 组件更新之前被调用
//在侦听器回调中访问的 DOM 将是被 Vue 更新之前的状态
//如果想在侦听器回调中能访问被 Vue 更新之后的 DOM，你需要指明 flush: 'post' 选项
//后置刷新的 watchEffect() 有个更方便的别名 watchPostEffect()

//停止侦听器
//侦听器必须用同步语句创建：如果用异步回调创建一个侦听器
//那么它不会绑定到当前组件上，你必须手动停止它，以防内存泄漏
//要手动停止一个侦听器，请调用 watch 或 watchEffect 返回的函数---unwatch()






</script>

<style scoped></style>
