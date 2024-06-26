<template>
    <div class="spine">
        <div id="bubble" class="bubble" v-show="bubbleVisible">
            {{ bubbleContent }}
        </div>
    </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import * as PIXI from 'pixi.js';
import { Spine } from 'pixi-spine';
// import skelUrl from '../assets/arona_spr.skel?url'
// import pngUrl from '../assets/arona_spr.png'
// import atlasUrl from '../assets/arona_spr.atlas?url'
import model from '../assets/'

const bubbleVisible = ref(false);
const bubbleContent = ref('Hello!');  // 自定义气泡内容
const talkAnimations = ['01', '02', '20']
onMounted(() => {
    const app = new PIXI.Application({
        backgroundColor: 0xFFFFFF,
        antialias: true,
        width: 500,
        height: 500,
        resolution: Math.ceil(window.devicePixelRatio),
        autoDensity: true,

    });
    document.querySelector('.spine')?.append(app.view)
    app.loader.add('arona', model, { metadata: { imageMetadata: { alphaMode: PIXI.ALPHA_MODES.PMA } } }).load(onAssetsLoaded)
    // app.loader
    //     .add('arona_skel', skelUrl, { metadata: { imageMetadata: { alphaMode: PIXI.ALPHA_MODES.PMA } } })
    //     .add('arona_atlas', atlasUrl, { metadata: { imageMetadata: { alphaMode: PIXI.ALPHA_MODES.PMA } } })
    //     .add('arona_png', pngUrl, { metadata: { imageMetadata: { alphaMode: PIXI.ALPHA_MODES.PMA } } })
    //     .load(onAssetsLoaded);

    function onAssetsLoaded(loader: any, res: any) {
        const arona = new Spine(res.arona.spineData)
        arona.position.set(app.screen.width / 2, app.screen.width / 2)
        console.log(arona.skeleton)
        arona.scale.set(app.screen.width / 3000)
        app.stage.addChild(arona)

        function triggerBlinkAnimation() {
            arona.state.setAnimation(1, 'Eye_Close_01', false)
            arona.state.addAnimation(0, 'Idle_01', true, 0)
            // entry.mixDuration = 0.5
            const randomInterval = Math.random() * (5000 - 2000) + 2000
            setTimeout(triggerBlinkAnimation, randomInterval)
        }
        triggerBlinkAnimation()

        function playTalkAnimation(spine: Spine, duration: number) {
            let elapsed = 0;
            const interval = (Math.random() + 1) * 100;  // 每200毫秒切换一次嘴型动画
            // spine.state.setAnimation(3, '12', false)
            function switchMouthAnimation() {
                const randomIndex = Math.floor(Math.random() * talkAnimations.length);
                const animationName = talkAnimations[randomIndex];
                spine.state.setAnimation(3, animationName, false);  // 在第3个轨道上播放说话动画片段
                elapsed += interval;
                if (elapsed < duration) {
                    setTimeout(switchMouthAnimation, interval);
                } else {
                    spine.state.setEmptyAnimation(3, 0);  // 清空第3个轨道上的动画
                }
            }
            switchMouthAnimation();
        }

        arona.interactive = true
        arona.buttonMode = true


        arona.on('pointerdown', () => {
            // arona.state.setAnimation(2, '25', false)
            // setTimeout(() => {
            //     arona.state.setEmptyAnimation(2, 0)
            // }, 2000)

            // 显示气泡
            bubbleContent.value = 'Hello!';  // 这里可以自定义气泡内容
            bubbleVisible.value = true;
            const bubble = document.getElementById('bubble');
            if (bubble) {
                const rect = app.view.getBoundingClientRect();
                // const pointerPosition =
                // bubble.style.left = `${rect.left + pointerPosition.x}px`;
                // bubble.style.top = `${rect.top + pointerPosition.y - 50}px`;  // 50 是气泡的偏移量，可以根据需要调整
            }

            // 隐藏气泡
            setTimeout(() => {
                bubbleVisible.value = false;
            }, 2000);  // 2秒后隐藏气泡

            playTalkAnimation(arona, 2000);
        })
    }
}
)

</script>

<style scoped>
.bubble {
    position: absolute;
    max-width: 150px;
    padding: 10px;
    background: rgba(255, 255, 255, 0.8);
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    font-size: 14px;
    line-height: 1.5;
    pointer-events: none;
    z-index: 1000;
}
</style>
