<template>
    <div class="canvas">
        <div class="canvas__topbar">

        </div>

        <canvas
            ref="canvas-body"
            class="canvas__body">
        </canvas>
    </div>
</template>

<script>

export default {
    data() {
        return {
            windowWidth: window.innerWidth,
            windowHeight: window.innerHeight,
            isDrawing: false
        }
    },
    computed: {
        canvas() {
            return this.$refs['canvas-body']
        },
    },
    watch: {
        windowWidth(changed) {
            this.canvas.width = changed
        },
        windowHeight(changed) {
            this.canvas.width = changed
        },
    },
    mounted() {
        this.canvas.width = this.windowWidth
        this.canvas.height = this.windowHeight

        this.$nextTick(() => {
            window.addEventListener('resize', this.resizeWindow);
        })

        this.canvas.addEventListener('mousedown', this.startDrawing)
        window.addEventListener('mouseup', this.stopDrawing)

        const context = this.canvas.getContext('2d')
        const canvasBounds = this.canvas.getBoundingClientRect()
        this.canvas.addEventListener('mousemove', (e) => this.draw(e, context, canvasBounds))
    },
    methods: {
        resizeWindow() {
            this.windowWidth = window.innerWidth
            this.windowHeight = window.innerHeight
        },
        startDrawing() {
            this.isDrawing = true
        },
        stopDrawing() {
            this.isDrawing = false
        },
        draw(event, context, canvasBounds) {
            if(this.isDrawing) {
                context.fillRect(event.pageX - canvasBounds.left, event.pageY - canvasBounds.top, 3, 3);
            }
        }
    }
}
</script>
