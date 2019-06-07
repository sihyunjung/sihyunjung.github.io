---
layout: post
title:  "연동확인"
date:   2019-06-02
categories: javascript
---

# Getting started with super simple vuejs

# 1. vuejs mount하기

1. tag id와 new 생성자에게 el id값을 넘겨주면 tag id와 연결된 vue 인스턴스가 생성된다.

    <body>
    <div id="app"></div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script>
    	new Vue({
        el: '#app'
      });
    <script>
    </body>

# 2. data binding하기

1. tag에 v-model을 attribute로 속성을 정의하면 vue인스턴스의 data메소드안에 정의된 property와 바인딩된다.

    <body>
    <div id="app">
    	<input type="text" id="user_id" v-model="userId">
    	<input type="password" id="user_password" v-model="userPassword">
    	{{ userId }}
      {{ userPassword }}
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script>
    		new Vue({
            el: '#app',
            data() {
                return {
                    userId: '',
                    userPassword: ''
                }
            }
        });
    </script>
    </body>

# 3. 리스트 만들기

1. vue생성자 안에서의 data메소드 안에서 items를 생성한다.
2. html tag안에서 temlate tag에서 v-for속성을 이용하면 간단하게 리스트를 생성이 가능하다
3. v-src대신 :src로 대신 사용가능하다

    <body>
    <div id="app">
        <ul>
            <template v-for="item in items">
                <li>
                    {{ item.id }}
                    <img :src="item.image">
                </li>
            </template>
        </ul>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data() {
                return {
                    items: [
                        {
                            id: 1, 
                            image: 'https://picsum.photos/210/118?image=1'
                        },
                        {
                            id: 2, 
                            image: 'https://picsum.photos/210/118?image=100'
                        },
                        {
                            id: 3, 
                            image: 'https://picsum.photos/210/118?image=160'
                        },
                        {
                            id: 4, 
                            image: 'https://picsum.photos/210/118?image=200'
                        },
                        {
                            id: 5, 
                            image: 'https://picsum.photos/210/118?image=210'
                        }
                    ]
                }
            }
        });
    </script>
    </body>

# 4. 리스트에 아이템 추가하기(event 관리하기)

1. vue생성자에 methods 메소드안에 이벤트와 연결할 메소드를 정의한다.
2. input tag에서는 v-model 속성을 사용하여 data메소드안에서 정의된 prototype을 추가하여 바인딩한다
3. button tag를 정의하여 @이벤트메소드 를 사용하여 methods안에 정의된 메소드와 바인딩한다

    <body>
        <div id="app">
            <ul>
                <template v-for="item in items">
                    <li>
                        {{ item.text }} / {{item.id}}
                    </li>
                </template>
            </ul>
            <input type="text" v-model="item.text">
            <button @click="addItem">추가</button>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
        <script>
            new Vue({
                el: '#app',
                methods: {
                    addItem () {
                        this.item.id = this.items.length;
                        this.items.push({ ...this.item });
                        this.item.id = '';
                        this.item.text = '';
                    }
                },
                data() {
                    return {
                        items: [
                            {
                                id: 0, 
                                text: 'https://picsum.photos/210/118?image=1'
                            },
                            {
                                id: 1, 
                                text: 'https://picsum.photos/210/118?image=100'
                            },
                            {
                                id: 2, 
                                text: 'https://picsum.photos/210/118?image=160'
                            },
                            {
                                id: 3, 
                                text: 'https://picsum.photos/210/118?image=200'
                            },
                            {
                                id: 4, 
                                text: 'https://picsum.photos/210/118?image=210'
                            }
                        ],
                        item: {
                            id: '',
                            text: ''
                        }
                    }
                }
            });
        </script>
    </body>

# 5. 컴포넌트 연결하기

1. Vue.component 사용
2. 사용하고자하는 템플릿 혹은 vue인스턴스와 연결된 tag안에서 정의된 컴포넌트 사용

    <!DOCTYPE html>
    <html>
    <body>
        <div id="app">            
            <ul>
                <template v-for="item in items">
                    <item :item="item"></item>
                </template>
            </ul>
            <input type="text" v-model="item.text">
            <button @click="addItem">추가</button>
        </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script>
        Vue.component('item', {
            props: [
                'item',
            ],
            template: `
                <li>
                    {{ item.text }} / {{item.id}}
                </li>
            `
        });
    
        new Vue({
            el: '#app',
            methods: {
                addItem () {
                    this.item.id = this.items.length;
                    this.items.push({ ...this.item });
                    this.item.id = '';
                    this.item.text = '';
                }
            },
            data() {
                return {
                    items: [
                        {
                            id: 0, 
                            text: 'https://picsum.photos/210/118?image=1'
                        },
                        {
                            id: 1, 
                            text: 'https://picsum.photos/210/118?image=100'
                        },
                        {
                            id: 2, 
                            text: 'https://picsum.photos/210/118?image=160'
                        },
                        {
                            id: 3, 
                            text: 'https://picsum.photos/210/118?image=200'
                        },
                        {
                            id: 4, 
                            text: 'https://picsum.photos/210/118?image=210'
                        }
                    ],
                    item: {
                        id: '',
                        text: ''
                    }
                }
            }
        });
    </script>
    </body>
    </html>