<template>
    <div class="carousel"
        @mouseenter="mouseEnter"
        @mouseleave="mouseLeave"
    >
        <div class="inner">
            <CarDot
                :hasDot="hasDot"
                :itemLength="itemLength"
                :currentIndex="currentIndex"
                :dotBgColor="dotBgColor"
                @dotClick="dotClick"
            />
            <CarDirector
                dir="prev"
                @dirClick="dirClick"
            />
            <CarDirector
                dir="next"
                @dirClick="dirClick"
            />
            <slot></slot>
        </div>
    </div>
</template>
<script>
import {reactive,toRefs,onMounted,onBeforeUnmount,getCurrentInstance} from 'vue'
import CarDot from './Dot'
import CarDirector from './Director'
export default {
    name:'Carousel',
    components:{
        CarDot,
        CarDirector
    },
    props:{
        autoplay:{
            type:Boolean,
            default:true
        },
        duration:{
            type:Number,
            default:3000
        },
        inital:{
            type:Number,
            default:0
        },
        hasDot:{
            type:Boolean,
            default:true
        },
        hasDirector:{
            type:Boolean,
            default:true
        },
        dotBgColor:String
    },
    setup(props) {
        const instance = getCurrentInstance()
        const state =  reactive({
            currentIndex:props.inital,
            itemLength:0
        })
        let t = null
        const autoplay = (()=>{
            if(props.autoplay){
                t = setInterval(()=>{
                   setIndex('next')
                },props.duration)
            }
        })
        const setIndex = ((dir)=>{
            switch(dir){
                case 'next':
                    state.currentIndex+=1
                    if(state.currentIndex===state.itemLength){
                        state.currentIndex = 0
                    }
                    break;
                case 'prev':
                    state.currentIndex-=1
                    if(state.currentIndex===-1){
                        state.currentIndex = state.itemLength-1
                    }
                    break;
                default:
                    break;
            }
        })
        const dotClick = (index)=>{
            state.currentIndex = index
        }
        const dirClick = (dir)=>{
            setIndex(dir)
        }
        const mouseEnter = ()=>{
            clearInterval(t)
            t=null
        }
        const mouseLeave = ()=>{
            autoplay()
        }
        onMounted(()=>{
            state.itemLength = instance.slots.default()[0].children.length
            autoplay()
        })
        onBeforeUnmount(()=>{
            clearInterval(t)
            t = null
        })
        return {
            ...toRefs(state),
            dotClick,
            mouseEnter,
            mouseLeave,
            dirClick
        }
    }
}
</script>
<style>
    .carousel{
        width: 100%;
        height: 100%;
    }
    .inner{
        position: relative;
        width: 100%;
        height: 100%;
        overflow: hidden;
    }
</style>