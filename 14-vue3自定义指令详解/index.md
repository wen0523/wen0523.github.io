# Vue3自定义指令详解


# Vue3自定义指令详解

## 一、自定义指令概述

### 1.1 什么是自定义指令

自定义指令是Vue3提供的强大功能，允许我们封装DOM操作的底层逻辑。当内置指令无法满足需求时，我们可以创建自定义指令来实现特定的DOM操作。

**使用场景：**
- 直接操作DOM元素
- 集成第三方库
- 实现特定的交互效果
- 封装常用的DOM操作逻辑

### 1.2 自定义指令的注册方式

**全局注册：**
```javascript
// main.js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

// 全局注册自定义指令
app.directive('focus', {
    mounted(el) {
        el.focus()
    }
})

app.mount('#app')
```

**局部注册：**
```vue
<template>
    <input v-focus />
</template>

<script>
export default {
    directives: {
        focus: {
            mounted(el) {
                el.focus()
            }
        }
    }
}
</script>
```

## 二、自定义指令的钩子函数

### 2.1 指令钩子函数

Vue3提供了7个钩子函数：

```javascript
app.directive('my-directive', {
    // 在绑定元素的父组件挂载之前调用
    created(el, binding, vnode, prevVnode) {
        console.log('created')
    },
    
    // 在绑定元素的父组件挂载后调用
    beforeMount(el, binding, vnode, prevVnode) {
        console.log('beforeMount')
    },
    
    // 在绑定元素挂载到DOM后调用
    mounted(el, binding, vnode, prevVnode) {
        console.log('mounted')
    },
    
    // 在包含组件的VNode更新之前调用
    beforeUpdate(el, binding, vnode, prevVnode) {
        console.log('beforeUpdate')
    },
    
    // 在包含组件的VNode及其子组件的VNode更新后调用
    updated(el, binding, vnode, prevVnode) {
        console.log('updated')
    },
    
    // 在绑定元素的父组件卸载之前调用
    beforeUnmount(el, binding, vnode, prevVnode) {
        console.log('beforeUnmount')
    },
    
    // 在绑定元素的父组件卸载后调用
    unmounted(el, binding, vnode, prevVnode) {
        console.log('unmounted')
    }
})
```

### 2.2 钩子函数参数

**el**：指令绑定的元素，可以直接操作DOM

**binding**：包含指令信息的对象
```javascript
binding = {
    id: string,           // 指令ID
    instance: Component,  // 使用指令的组件实例
    value: any,          // 传递给指令的值
    oldValue: any,       // 之前的值（仅在update和beforeUpdate中可用）
    arg: string,         // 传递给指令的参数
    modifiers: object,   // 修饰符对象
    dir: object          // 指令定义对象
}
```

**vnode**：绑定元素的底层VNode

**prevVnode**：之前的渲染中绑定元素的VNode（仅在update和beforeUpdate中可用）

## 三、自定义指令的创建和使用

### 3.1 简单自定义指令

**自动聚焦指令：**
```javascript
// 全局注册
app.directive('focus', {
    mounted(el) {
        el.focus()
    }
})

// 使用
<template>
    <input v-focus placeholder="自动聚焦的输入框">
</template>
```

**权限控制指令：**
```javascript
app.directive('permission', {
    mounted(el, binding) {
        const { value } = binding
        const userPermissions = ['admin', 'editor']
        
        if (!userPermissions.includes(value)) {
            el.style.display = 'none'
            // 或者 el.remove()
        }
    },
    
    updated(el, binding) {
        const { value } = binding
        const userPermissions = ['admin', 'editor']
        
        if (!userPermissions.includes(value)) {
            el.style.display = 'none'
        } else {
            el.style.display = ''
        }
    }
})

// 使用
<template>
    <button v-permission="'admin'">管理员按钮</button>
    <button v-permission="'editor'">编辑按钮</button>
</template>
```

### 3.2 带参数的自定义指令

**防抖指令：**
```javascript
app.directive('debounce', {
    mounted(el, binding) {
        const { value, arg = 300 } = binding
        let timer = null
        
        el.addEventListener('click', () => {
            if (timer) {
                clearTimeout(timer)
            }
            
            timer = setTimeout(() => {
                value()
            }, parseInt(arg))
        })
    },
    
    unmounted(el) {
        // 清理事件监听器
        el.removeEventListener('click')
    }
})

// 使用
<template>
    <button v-debounce:500="handleClick">防抖按钮</button>
</template>

<script>
export default {
    methods: {
        handleClick() {
            console.log('按钮被点击了')
        }
    }
}
</script>
```

**复制到剪贴板指令：**
```javascript
app.directive('copy', {
    mounted(el, binding) {
        const { value } = binding
        
        el.style.cursor = 'pointer'
        el.addEventListener('click', async () => {
            try {
                await navigator.clipboard.writeText(value)
                alert('复制成功！')
            } catch (err) {
                console.error('复制失败:', err)
                alert('复制失败，请手动复制')
            }
        })
    },
    
    updated(el, binding) {
        // 更新复制的值
        const { value } = binding
        el.removeEventListener('click')
        el.addEventListener('click', async () => {
            try {
                await navigator.clipboard.writeText(value)
                alert('复制成功！')
            } catch (err) {
                console.error('复制失败:', err)
                alert('复制失败，请手动复制')
            }
        })
    },
    
    unmounted(el) {
        el.removeEventListener('click')
    }
})

// 使用
<template>
    <span v-copy="'要复制的文本'">点击复制</span>
    <span v-copy="message">点击复制: {{ message }}</span>
</template>
```

### 3.3 带修饰符的自定义指令

**懒加载图片指令：**
```javascript
app.directive('lazy', {
    mounted(el, binding) {
        const { value, modifiers } = binding
        
        // 创建观察器
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    const img = entry.target
                    img.src = value
                    
                    // 如果有fade修饰符，添加淡入效果
                    if (modifiers.fade) {
                        img.style.opacity = '0'
                        img.style.transition = 'opacity 0.5s'
                        
                        img.onload = () => {
                            img.style.opacity = '1'
                        }
                    }
                    
                    // 停止观察
                    observer.unobserve(img)
                }
            })
        }, {
            threshold: 0.1
        })
        
        observer.observe(el)
    },
    
    updated(el, binding) {
        // 更新图片URL
        const { value } = binding
        if (el.src !== value) {
            el.src = value
        }
    },
    
    unmounted(el) {
        // 清理观察器
        if (el._observer) {
            el._observer.unobserve(el)
        }
    }
})

// 使用
<template>
    <img v-lazy="'image1.jpg'" alt="图片1">
    <img v-lazy.fade="'image2.jpg'" alt="图片2（带淡入效果）">
</template>
```

## 四、函数式自定义指令

Vue3支持更简洁的函数式自定义指令：

```javascript
// 函数式指令（仅包含mounted和updated逻辑）
app.directive('color', (el, binding) => {
    el.style.color = binding.value
})

// 使用
<template>
    <p v-color="'red'">红色文本</p>
    <p v-color="textColor">动态颜色</p>
</template>

<script>
export default {
    data() {
        return {
            textColor: 'blue'
        }
    }
}
</script>
```

## 五、自定义指令的最佳实践

### 5.1 性能优化

**避免频繁操作DOM：**
```javascript
// 不好的做法
app.directive('highlight', {
    updated(el, binding) {
        // 每次更新都操作DOM
        el.style.backgroundColor = binding.value
    }
})

// 好的做法
app.directive('highlight', {
    mounted(el, binding) {
        el.style.backgroundColor = binding.value
    },
    updated(el, binding) {
        // 只有值变化时才更新
        if (binding.value !== binding.oldValue) {
            el.style.backgroundColor = binding.value
        }
    }
})
```

### 5.2 内存管理

**清理资源：**
```javascript
app.directive('resize', {
    mounted(el, binding) {
        const callback = binding.value
        el._resizeObserver = new ResizeObserver(callback)
        el._resizeObserver.observe(el)
    },
    
    unmounted(el) {
        // 清理观察器
        if (el._resizeObserver) {
            el._resizeObserver.disconnect()
            delete el._resizeObserver
        }
    }
})
```

### 5.3 错误处理

**添加错误处理：**
```javascript
app.directive('safe-html', {
    mounted(el, binding) {
        try {
            const { value } = binding
            
            // 简单的HTML清理（实际项目中应该使用专门的库）
            const cleanHtml = value
                .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
                .replace(/javascript:/gi, '')
                .replace(/on\w+="[^"]*"/g, '')
            
            el.innerHTML = cleanHtml
        } catch (error) {
            console.error('safe-html指令错误:', error)
            el.textContent = '内容加载失败'
        }
    },
    
    updated(el, binding) {
        try {
            const { value } = binding
            if (value !== binding.oldValue) {
                const cleanHtml = value
                    .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
                    .replace(/javascript:/gi, '')
                    .replace(/on\w+="[^"]*"/g, '')
                
                el.innerHTML = cleanHtml
            }
        } catch (error) {
            console.error('safe-html指令错误:', error)
            el.textContent = '内容加载失败'
        }
    }
})
```

## 六、实际应用案例

### 6.1 拖拽指令

```javascript
app.directive('draggable', {
    mounted(el, binding) {
        const { modifiers } = binding
        let isDragging = false
        let startX, startY, initialX, initialY
        
        const handleMouseDown = (e) => {
            isDragging = true
            startX = e.clientX
            startY = e.clientY
            
            const rect = el.getBoundingClientRect()
            initialX = rect.left
            initialY = rect.top
            
            el.style.position = 'fixed'
            el.style.zIndex = '1000'
            el.style.cursor = 'grabbing'
            
            document.addEventListener('mousemove', handleMouseMove)
            document.addEventListener('mouseup', handleMouseUp)
        }
        
        const handleMouseMove = (e) => {
            if (!isDragging) return
            
            const deltaX = e.clientX - startX
            const deltaY = e.clientY - startY
            
            // 如果有boundary修饰符，限制在父容器内
            if (modifiers.boundary) {
                const parent = el.parentElement
                const parentRect = parent.getBoundingClientRect()
                
                const newX = Math.max(0, Math.min(initialX + deltaX, parentRect.width - el.offsetWidth))
                const newY = Math.max(0, Math.min(initialY + deltaY, parentRect.height - el.offsetHeight))
                
                el.style.left = newX + 'px'
                el.style.top = newY + 'px'
            } else {
                el.style.left = (initialX + deltaX) + 'px'
                el.style.top = (initialY + deltaY) + 'px'
            }
        }
        
        const handleMouseUp = () => {
            isDragging = false
            el.style.cursor = 'grab'
            
            document.removeEventListener('mousemove', handleMouseMove)
            document.removeEventListener('mouseup', handleMouseUp)
        }
        
        el.style.cursor = 'grab'
        el.addEventListener('mousedown', handleMouseDown)
        
        // 保存事件处理器以便清理
        el._dragHandlers = {
            mousedown: handleMouseDown,
            mousemove: handleMouseMove,
            mouseup: handleMouseUp
        }
    },
    
    unmounted(el) {
        if (el._dragHandlers) {
            el.removeEventListener('mousedown', el._dragHandlers.mousedown)
            document.removeEventListener('mousemove', el._dragHandlers.mousemove)
            document.removeEventListener('mouseup', el._dragHandlers.mouseup)
        }
    }
})

// 使用
<template>
    <div v-draggable class="drag-box">可拖拽的盒子</div>
    <div v-draggable.boundary class="drag-box">限制在父容器内拖拽</div>
</template>

<style>
.drag-box {
    width: 200px;
    height: 100px;
    background-color: #4CAF50;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    user-select: none;
}
</style>
```

### 6.2 无限滚动指令

```javascript
app.directive('infinite-scroll', {
    mounted(el, binding) {
        const { value } = binding
        const threshold = binding.arg || 100
        
        const handleScroll = () => {
            const { scrollTop, scrollHeight, clientHeight } = el
            
            if (scrollHeight - scrollTop - clientHeight < threshold) {
                value()
            }
        }
        
        el.addEventListener('scroll', handleScroll)
        el._scrollHandler = handleScroll
    },
    
    unmounted(el) {
        if (el._scrollHandler) {
            el.removeEventListener('scroll', el._scrollHandler)
        }
    }
})

// 使用
<template>
    <div v-infinite-scroll:200="loadMore" class="scroll-container">
        <div v-for="item in items" :key="item.id" class="item">
            {{ item.name }}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            items: [],
            page: 1
        }
    },
    methods: {
        loadMore() {
            // 模拟加载更多数据
            const newItems = Array.from({ length: 10 }, (_, i) => ({
                id: this.items.length + i,
                name: `项目 ${this.items.length + i}`
            }))
            
            this.items.push(...newItems)
            this.page++
        }
    },
    mounted() {
        this.loadMore()
    }
}
</script>

<style>
.scroll-container {
    height: 400px;
    overflow-y: auto;
    border: 1px solid #ddd;
    padding: 10px;
}

.item {
    padding: 10px;
    border-bottom: 1px solid #eee;
}
</style>
```

## 七、自定义指令的测试

### 7.1 单元测试

```javascript
// directive.test.js
import { nextTick } from 'vue'
import { mount } from '@vue/test-utils'

describe('v-focus directive', () => {
    it('should focus element when mounted', async () => {
        const wrapper = mount({
            template: '<input v-focus />',
            directives: {
                focus: {
                    mounted(el) {
                        el.focus()
                    }
                }
            }
        })
        
        const input = wrapper.find('input')
        const focusSpy = vi.spyOn(input.element, 'focus')
        
        await nextTick()
        
        expect(focusSpy).toHaveBeenCalled()
    })
})

describe('v-permission directive', () => {
    it('should hide element without permission', async () => {
        const wrapper = mount({
            template: '<div v-permission="\'admin\'">管理员内容</div>',
            directives: {
                permission: {
                    mounted(el, binding) {
                        const userPermissions = ['user']
                        if (!userPermissions.includes(binding.value)) {
                            el.style.display = 'none'
                        }
                    }
                }
            }
        })
        
        const div = wrapper.find('div')
        expect(div.element.style.display).toBe('none')
    })
})
```

## 八、总结

### 8.1 自定义指令的优势

1. **代码复用**：将常用的DOM操作逻辑封装成指令
2. **声明式语法**：使模板更加简洁和易读
3. **解耦逻辑**：将DOM操作与组件逻辑分离
4. **性能优化**：减少重复的DOM操作代码

### 8.2 使用建议

1. **合理使用**：不要过度使用自定义指令，优先考虑组件和组合式函数
2. **性能考虑**：注意内存管理和性能优化
3. **错误处理**：添加适当的错误处理机制
4. **文档说明**：为自定义指令提供清晰的文档

### 8.3 最佳实践总结

- 使用函数式指令简化简单逻辑
- 注意在unmounted钩子中清理资源
- 添加适当的错误处理
- 编写测试确保指令的可靠性
- 提供清晰的文档和使用示例

---

**自定义指令是Vue3生态系统中的重要组成部分，掌握它们将使你能够构建更加灵活和强大的Vue应用。通过合理使用自定义指令，你可以大大提高代码的复用性和可维护性。**
