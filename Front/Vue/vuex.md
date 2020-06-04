# vuex

[TOC]

### 1.什么是vuex

   - ### Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

        - 状态管理：对数据的一种统一管理方式；对于数据都已状态方式进行管理
        - 实际上就是vue的官方



### 2.vuex的项目装载

- 通过vue的自动装载，选择vuex功能，项目会自动安装vuex模块
- 项目src目录下会存在一个store.js文件，该文件为数据管理文件

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

//创建vuex中的所有组件可以直接调用的数据管理对象
export default new Vuex.Store({
    state:{

    },
    mutations:{

    },
    actions:{

    },
    actions:{

    }
})
```







### 3.项目中vuex的使用

1. **配置属性state**

   用于定义项目中被管理和使用的数据变量

   ```js
       state:{
           //key:value
           msg:"vuex.msg",
           arr:[1,2,3]
       },
   ```

2. 

士大夫







### 4.组件中使用数据仓库

1. 组件从仓库加载数据

   ```html
   <p>{{msg}}</p>
   ```

   依赖于vuex为所有组件提供的$store仓库进行仓库数据的获取

   ```js
   //组件.js的export default 
   computed: {
           msg(){
               return this.$store.state.msg
           },
           arr(){
               return this.$store.state.arr;
           }
       },
   ```

   vuex中提供一个特殊的辅助方法，用于引入

   ```js
   import {mapState} from 'vuex'
   
   /*mapState(option)从vuex.store中进行数据获取
       option描述需要获取的数据变量
           取值为rray
           数组照片你还没一盒元素都是需要获取的变量名称
       mapState返回一个 对象那个，对象中通过封装方式为每个变量定义一个获取方法
       {
           info(){
           return this.$store.state.msg
       },
       arr(){
           return this.$store.state.arr;
           }
       }
   */
   console.log(mapState(['msg','arr']))
   export default {
       mounted(){
           // console.log(this.$store.state.msg)
       },
       computed: {
           ...mapState(['msg','arr'])
       }
   }
   ```

   ==三点函数==

   <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAUsAAADOCAYAAAC3vSBBAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAAEnQAABJ0Ad5mH3gAABkMSURBVHhe7Z3Njx1HuYf9D7BDSERs4hVigxRxd16AlNxwuRtbkbgyuZs4C0I2WCzCgkSK8qHEShSwDIpMQDaYkMQoESAZGzt85WNsxUR28Fxff8y9McTmxtgzGEMmE41DXf3a/Y7fU1PdXeeje86ZeR6p1N1V1XX6dHU981b3OWfWBQAAaARZAgBkgCwBADKoleXCwkK5BuPG3r17w/r168PWrVvD/Px8mQvQH3Ydbdq0KczOzpa5kKJSlidPngw7d+4Mly5dKnNgXDh79mzYuHFjsZxE/jb1enj94x8rln69S8bhGEbBNx96N3xi3VtFuvP2/w1zl6+VJaFYV56VK73x2j/K0l62bdtWJKimVpY7duxAmGPIkSNHJjqilJR+/5lbwvyZ0z3rnos/fjacvvee8OH8e2XOaMk5hnFH4pMsDa1//cvvhPfn/1nm9KL6sVCNSb+muqB2Go4wxxNNnSb5wpaU3vq324ulX/e0LcucY5g0Zk4thC/96/8UyxR15ciymcYHPMMK8+0fTodffe758MHlG52gdeWpzPjDA6+Fn3zkW0vpL6+eL0tCuHpqNrx6x0/D1dNzxX4qj9usY27ug7B582/DG2/cOH6tK09lxlNPTYebbnp+Kb344rmy5Drax5f79tTOV74yFU6c+Gu4776jRfmttx4IMzNXyxqjo0qWdsH/+te/Lu5Dpe5pal15Vr5hw4al6bzt/8ADDxRlu3btWqqrMkPrtn9cZlgdHWs/SJKaEscpjvzietoWkqsk++dnvlvs89YXPh8u//xnRZ1Ryzc+D/E0Ni5X8uc77ou4r4TVybmn2CTLl37018rIU8eaen24QaMsxaFDhwph7t69u8zJx8To5ad1Lztt+/K3I8FKlvs+9f0eiUquSjnkyFJilOTef3/5FEWovpefltq2Nu01vGSb2vTMz70bXtjyybDztnXL0jtHD5S1rqNBmZKQDU676DW4NMh83X379vXc61RbVt8LTknryvP3s7TtB7yW2la+x7c1CHWRpcokwcXZy8W2ltpWvslS2/NnzxRLbf/9+LGRRo86t348xOchte3vM5sEvWB9XxhWL0eWqWm4xHnbp08n72l64uOD5bQeWYpYbFqXEKuQHF/+7HPF0rYly1ioR7bsD9feWyxzqsmVZV0kqKgzjjSVpyTsNXyd+DWGxQsshfLjQeVFl8Lv49f1WjZwtW5taBkLsOk1BqFKliZDiyQNqy9xWrlJVPcktX7yzs2tTbVNanZudC69+Kzc+k5S2rJlS09fDSMs3Y+se4AjTJxVdewY/XHDDTq5Z2nTaEWKSsWUuhShsOjTT8MlRy9LL89+yZGlkOhsiu3LFBna1DpOsSz9a/TDqCLLJllqICrikXQt5crSBpPf11JXsvQC9Ghb+R+c/1NnstR7js+D9YudZ5Ojlto2EWo73lfJ18nFRKlpdhOKPv1DIUOvSWRZT6UsR/lwR9GfokCbbvso08p8XizHrmTpkQT9FDoVWXqGlWU/eJF5mmSpfJV70fp9/HpKlkJLv39bVMlyXCJLnQN/ru0PifKv/uPvS0kC9NuGpBRHloNg0WKOKIVEmaqrvk9dU3CDWlmO8im4SVLJT6ctqrRpuclzlJGlRYYmO7vfWCdL1fWylATrpumTIEsNUB/tqJ7q9yNL1cmJflRPotC+gyDBVX2cRyKUBCVAoTqqq328TIeVZXx+PDofvg/0Pv37tXPmBekxudp5rcLqpY7BPkeZihRTKAKVWFMPgNRfqWsKblA7Db92rfnBRC4mxdR9RsnTT8HfeelMz1R9WFkKScymziY1Pb02WSpy9NPrlEh9G5ZMjpMgS2GD2qZ8euBjEU6OLIWJ0CfleayO9h2Uc488tPS0OxanL1OSEMUoZSn0vlPvQ+dI58rev51nq2flFl1a8u2YCH153K9WJyVLSdI+bO6TydOm55bqHvCov1LXFNyg8QEPjB9c2N1h0o//GDRh8vSRpdoY5J5kF3BNNYMsJxANNm7Gt09dVFeH7RfLUlFlv211hckdqkGWE4oGXmraBqPBIspBz29qGj6OorTraFwlPk4gS4AWqXrAA5MHsgRoEWS5ekCWAC2CLFcPyBKgRZDl6gFZArQIslw9IEuAFkGWqwdkCdAiyHL1gCwBWgRZrh6QJUCLIMvVA7IEGDESpKXUNkwmyBIAIINGWS4uLo7dd4/n5ubCE088Ed58880yJ5+ZmZnw6KOPhgcffLBI+/fvL0uus2fPnqJtvUbb6LWffvppvtsNMAHUyvLq1avh8OHDRZI0x4VBZWn7xYL0IEsASFEpS4lS/1L14MGD4Y9//GOZOx4MKktFldpPy3EAWQJMDklZelFeuHChzL2e32+EmRKb1i16kygkjKmpqWJp0+NYhNq2slSdpum1qJOl6tu+dmwelSvq9PW07Wk6BnuvVq6ELAEmg2WylCRSotS68o4dO1bm5JErS4nD6kgyXljKl4RMcnGbJinbtnKTlRecT/41DH9sHmvDBBm/ZtMx2Pv0glUZsgSYDJbJsmr6ffHixSLv1KlTZU4esdiEF5JJxKQiJB6VmxwtojPiNlUWR3kpEcXtpvDH5onbi4+76Rj0mtu3b+9pN3WMADCeJKfhGryaFkuO09PX/+vioAwry1R53KYkFUeNSrGI2pRl0zGk2kWWAJND5QMe3ZschTC7kKXK4qguRZuybDoGZAkw2VTKUpgwlQb96FAsFYlK9/ZMHCkZxlLzUjFRKmozWVqbtl1F3G6KQWXZdAx23Fau/XzkCQDjTa0shSQ5qCgNCcKmpSYMu3+XI0urY23Yk3MvJpOV1VHybYq4XSNu35Las7pNshRNx+DPg/a192FtAsD40ihLAABAlgAAWSBLAIAMkCUAQAbIEgAgA2QJAJABsgQAyGDdlStXAolEIpHqE5ElAEAGyBIAIANkCQCQAbIEAMgAWQIAZIAsAQAyQJYAABnUynJhYaFcg3Fj7969Yf369WHr1q38HiZAB1TK8uTJk2Hnzp3h0qVLZc7q5f33r4X77jsaXnzxXJlTz8zM1XDrrQey64+as2fPho0bNxZLAOiGWlnu2LFjTQhz0mR55MgRIkqAjqmdhq8VYfYry5VGU3BkCdAtjQ94hhXm2z+cDr/63PPhg8s3BrbWlacy4w8PvBZ+8pFvLaW/vHq+LAnh6qnZ8OodPw1XT88V+6k8brOJubkPwubNvw033fR8kbSuPGGy3LNnplhaHS9PrVt+qrxLkCVA9zTKUhw6dKgQ5u7du8ucfEyMXn5a97LTti+PBStZ7vvU93skKrkq5WCijOVnwjRZSoBvvHH9D4KWmmpryh0zSCR6eeZ4+MEdHw07b1vXk5Snsn7Ytm1bIUwA6I7WI0sRi03rPqqMkRxf/uxzxdK2JctYqEe27A/X3mv+z5MSn48khQlUZSn5WZ7J0zOILEeBPQHXPUsA6JZO7lnaNFqRolIxpS5FKCz69NNwydHL0suzXyQ1yU2SM7zw6mSZEuIgsiSyBJhsKmU5yoc7iv4UBdp020eZVubzYjkOK8tBIktfHjOILEcJ9ywBuqdWlqN8Cm6SVPLTaYsqbVpu8hxlZGni83J76qnpQngSX0p+Wo8FayBLgLVH7TT82rUb09ZhMSmm7jNKnn4K/s5LZ3qm6sPKUthnI+1JtonSkDytLFXuQZYAa4/GBzywnJWWJR9KB+geZJnJgQPnlyJN3ces+lhRF/B1R4DuQZaZ+Gn8SorSsI8REWECdAOyBADIAFkCAGSALAEAMkCWAAAZIEsAgAyQ5Rgxf+Z0+P1nbgkXf/xsz3qXjMMxAIwjyHKMMDn9ber1nvUuGYdjABhHkOUYsTh7Obz1hc8XcvLrXTIOxwAwjqwZWeqD2/oAtz7IrbRhw4Zl34DRT59ZuVL8u5Ha9uWq71F7atfKUx8Yn52dDZs2bRr4w+QS1+sf/9hSOvfIQ2XJdSwatPLT994TPpx/rywNxbryrFx1tQ8A1LNmZLlv374eOUp0XlgSoZejviEjqUluQkv/S/EmRttH7ezatWupPZOi2vEMI0tFen/+3jPlVu+UWUiEf37mu0tytMjQ33O8/POf9chRso2FCgDLWbPTcEnOyzBGMqz7/rVEJ+HFMvRIyHH0OUosSqx7ACMZxtGnR6KVUCVWAKhmzcgyniIrxZGjtn15PFWPp+lKXpb2fW2fRi1Lic+m0Ja8LLUel3tZxtN0JWQJ0MyakGVqSuwjS4sSvdjiyFL7ernGkaXaS8l1lLKUCL3Y4shSUWJ8D9JHlqlpOZElQB5rQpYWVdr9RZOnyS+WqYnQy0/S8/cZLYq0fWKZ6rVSkaW9lm8rF0nP31+0KNLkF8vUHgaZLON7nCbPQWSp96X3Z+cUYLWzZqbhJjclSVAPfLZs2bJMbpas3GRpkrNyixpjwVq51vXAZ5SyNLnZ9NmiRpOlRZpWrnU98DFZChOsksSpBz4n79zctyztfA3yPgAmkTX7gAeGw6Qf/zEAWK0gSxgIi9SZhsNaAVlCX1hEGT/MAljtIEsAgAyQJQBABsgSACADZAkAkAGyBADIAFkCAGSALAEAMkCWAAAZIMsRoW+y6MPa9l1zYd8fN+Lvj6e+AaP6Vha3pw+B6/vqMzMzRVmqDgC0A7IcETmyjLdjVOZ/mCL+JSP79SQv2aY2AWA0IMsRkSvLqkhQef5XjoTyVN/EaLL00aiEyi//ALQPshwRObIU2rZptpecjxrj5GVZ968uAKA9kOWIyJWlYfcvrTwVWcYgS4CVY03J8vLM8fCDOz4aXtjyyTA/926ZewPlqUx1VDfFK9+6J+y8bV048dL2Muc6scjsJ8yqZClimWq9bkqNLAFWDmTpGEaWQrKzqbPWJUyToUWSVq6UEqNvQ8lHq8gSYOVgGg4AkAGyBADIAFkCAGSALAEAMkCWAAAZIEsAgAyQJQBABp3I0j6gXfW9aACAcafTyFIfuLYPaQMATBKdylLfn677Oh8AwLiCLAEAMkCWAAAZdCpLfggCACaVTmUp7Nd3iDABYJIgsgQAyIB7lgAAGSBLAIAMkCUAQAbIEgAgg05lydcdAWBS6USW/JAGAEw6nUaWAACTCrIEAMgAWQIAZIAsAQAyQJYAABkgSwCADJAlAEAGyBIAIANkCQCQAbIEAMggS5aLi4vlGgDA2qRRltPT00UaJ+bm5sITTzwR3nzzzTInn5mZmfDoo4+GBx98sEj79+8vS66zZ8+eom29RtvotZ9++ml+hQlgAqiVpSR58ODBMDU1NVbR5aCytP1iQXqQJQCkqJTluIpSDCpLRZXaT8txAFkCTA5JWaZEqQF97NixcPHixWI7l5TYtG7Rm9qVMPRaWtr0OBahtq0sVadpei3qZKn6tq8dm0flijp9PW17mo7B3quVKyFLgMlgmSwvXLhQiFLJD2KJQHlHjx4tc/LIlaXEYXUkGS8s5UtCJrm4TZOUbVu5ycoLzif/GoY/No+1YYKMX7PpGOx9esGqDFkCTAbJyPLEiROFGA8fPtxZZGlSERKPyk2OFtEZcZsqi6O8lIjidlP4Y/PE7cXH3XQMes3t27f3tJs6RgAYTyrvWaaEOQjDyjJVHrcpScVRo1IsojZl2XQMqXaRJcDkUClLMQphdiFLlcVRXYo2Zdl0DMgSYLKplaWQMJUGJZaKRKV7eyaOlAxjqXmpmCgVtZksrU3briJuN8Wgsmw6BjtuK9d+PvIEgPGmUZZi2I8OSRA2LTVh2P27HFlaHWvDnpx7MZmsrI6Sb1PE7Rpx+5bUntVtkqVoOgZ/HrSvvQ9rEwDGlyxZAgCsdZAlAEAGyBIAIANkCQCQAbIEAMgAWQIAZIAsAQAyWHflypVAIpFIpPpEZAkAkAGyBADIAFkCAGSALAEAMkCWAAAZIEsAgAyQJQBABrWyPH/+fLkGALC2qZTlK6+8Enbs2BFOnjxZ5rSDfvh269atYe/evWVOPWfPng0bNmzIrg8AMAoqZXnp0qWwc+fO1oWJLAFgEqidhkuYu3fvLoSpSLMN+pUlAMBK0PiAZ2FhITz33HOFMA8dOlTm9sfs7GzYtGlTWL9+fZG0rjxhsty1a1extDpenlq3/FQ5AEDbNMpy2Om4iTKWnwnTZCkBHjlypCjXUlNtTbljiEQBYCVonIYPe99S4vORpDCBqiwlP8szeXqQJQCsBJWylBwlSqVhHvBIapKbJGd44dXJMiVEZAkAK0GjLBVdDsMgkaUvj0GWALAS1E7Dr127Vq4NjonPy23btm2F8CS+lPy0HgvWQJYAsBI0PuAZBfbZSHuSbaI0JE8rS5V7kCUArASdyHKUIEsAWAkmQpb79u1bijR1H7PqY0UAAG0xEbL003hECQArwcRNwwEAVgJkCQCQAbIEAMgAWQIAZIAsAQAyQJYAABkgSwCADJAlAEAGyBIAIANkCQCQQZYsFxcXyzUAgLVJoyynp6eLtNLY98Nvvvnm5I8CAwD0Sz9eqZWlJHnw4MEwNTU1dHR54MCB8Mtf/rLc6g/9CPDGjRvDCy+8UOasPt5+5KFw/N8/HxZnL/esd8k4HMMk8O5zz4aj/3JLmD9zume9S8bhGMaBl340Fz6x7thSeuO1f5Ql1/nmQ//XU67tFHLLV7/61crf0RWVshylKIVk+bWvfS08+eST4fz582VuHjJ+0xuZZD6cfy+cvveeQlB+vUvG4RgmBZ0XnR+dJ7/ukcBS+aMi5xhWOxLj17/8p/D+/D+LbYnzzttnwtzl9H94mDm1EG779H8vE6rIcUxSlilRqpFjx46FixcvFtv9YrK09Lvf/a4saUbWf/zxx8ut1YeiN0Vxf5t6vWe9S8bhGCYBCUlikgz9eowElsofBbnHsNaQBCVDSTFFnSxzHLNMlhcuXChEqeQtOzMzU+QdPXq0zOmPWJZK3/nOd5L/Z8ejY5Dxq6bgx48fD1/84hd70i9+8YuyNIRz586Fu+++u6f8mWeeKUtDWFhYCI899lhPuX6J/cqVK2WNUNT35WpP7Xp0fG3eT9Xge+2mjy2leNolsflyJT+AVFf7+HK12RXqE51nnZ+q81zXF9pf/WB9Ydta1zVgxP3t+9rQMaivqq6pQbE/Mv4cp8513Bc+KlS9C9/7bpGnsrmXDxXrXU+zRzEuuu6LushS0aei0KppuETZdAzJyPLEiROFGA8fPrwsspybmyu2+yUlS6WHH364Vpgqu+uuu5I/+KsB4ztIHakOtcFjIrVtuwBMplbfd6INatW1+rYt1FZ80Yi2ZGmRgx9QEqMGpQankBT9YIojQxOpba9ENKLzqr6wcxf3VVNfaP0b3/hGUW5taWnbQm3VXQ9GW7I01A/Hv3B7Um7qg1RfWV9IltNf+o9i2/5AmjCt/9qmqS9yxkXXfWFRo4Tp8fc04zLPwLIUKWEOQ0qW3/72tytFaU+p9GAnVSd18tUxikK1tA61gSTiffwFYOgCsYskvgBEap820QDxYhQaSCbPWIzCD9aUGFP7tI3OqT+X/faFllZu62pDS7URtyd0HWjA+rwu0Hn1f9yMqvMuKSpZX8Xr2u+//nNzUr5tMOy46LovFEkqoqyKGoVFlv4ep8dmsHX/iaFSlmKUwhz0nmWV8Zs6zIvT8PuojupqHyPuZH+BiNQ+bWMDyYjl1yRTL04jtU+bpM6b+uX+++/P6gtf7vO1rjbUlrZtuueT9WWXxH1mVJ131VWfeZn6flOSLLvor1GMiy77wkRZFzUaVdN0yVFBWZUkjVpZCglTaVhMlv0+DZcoUzde1THqIHWUsA6t+utnHWr7xBeAUJs2dUhdNHYRtNHpKWIxCg0kP6X2YhQ24GywxgPU2vT7tI2XmqFzmNsXfn8lk6xvw6+vJKk+M1KylAg1LTdBmhS1bX2ktlLybYNRjIuu+qJq6l2FIs9U9FnlmJhGWYpRTcOV+iUnsrQOVodZJ6pj4ym5yk2mcadrqXJrU/i/oHZB2EXj0fG1cb9S+ChFA0ii9Pe8/AA0UaqODVbVi6fkKu938Nk0peq2SB2+rwx/bpv6okqQquf7J9U3MW3fr6yTpRejsP7y/ZsSpJap9uoY9Jocxbjooi/0RFv3IVNPtlNIqFVPynPuV4osWa4UNkBTHe4FqKQOUgdaJwttW7ny/eAS1tFKauvUqVNLg1Kos9XpKtfFYoM2vgh0stv6r5M2wCQ4Daz5s2d67l95ASppwMWDS9tWrnw/EHPJuadThc67CU7EA1LU9YXvN78e97fWrQ1LatfT5h82w/6o+T4xdO59WdxPtm3r1r++jRzsfQ4ion7HxW9+85tlfwzb7AubetuDG59MnoogfX7TvcqcYxhrWebeS4Bu0B8FXVi6wGC80eBv6w94jP5o+SBkkujHMWMvS3V4m1EA5KE+aDsig9Gg2yQSQM59uH6x2VUcZcZR46TQTwAw1rIUFqoT0awMNvC6ilJgODT4B51+56Iosm56PQkM4pWxlyUAwDiALAEAMkCWAAAZIEsAgAzW6akWiUQikeoTkSUAQAbIEgAgA2QJAJABsgQAyABZAgA0EsL/A1RW8SUVbI2KAAAAAElFTkSuQmCC" alt="三点函数" style="zoom:0%;" />

2. 修改属性

   一般方法

   ```html
    <input type="text" v-model="msg"></input>
   ```

   

   ```js
     computed:{
          msg:{
               get(){
                   return this.$store.state.msg
               },
               set(nv){
                   /*
                   组件触发mutation方法，通过this.&store.commit方法触发
                   this.$store.commit(name,nv)
                       name:方法名
                       nv:参数
                   commit只能用两个参数方法的多个参数就要用对象
                   */
                   this.$store.commit("setinfo",nv);
               }
           },
     }     
   ```

   功能性拆分

   ```html
     <input type="text" :value="msg" @input="setInfo($event.target.value)">
   ```

   ```js
       methods:{
           ...mapMutations(["setInfo","pushArr","setArr"])
       },
   ```

3. 页面调用异步方法