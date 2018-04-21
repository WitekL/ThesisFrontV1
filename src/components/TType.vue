<template>
  <v-stage :config="configKonva" ref="stage">
    <v-layer ref="layer">
      <div class="ttype">
        <v-line :config="topLine"></v-line>
        <v-rect :config="snapTopLeft" ref="snap"></v-rect>
        <v-rect :config="rect" v-on:click="referee" ref="rekt" @dragstart="drag" @dragmove="dragmove" @dragend="dragend"></v-rect>
        <v-line :config="middleLine"></v-line>
        <v-line :config="bottomLine"></v-line>
      </div>
    </v-layer>
    <v-layer ref="tmpLayer"></v-layer>
  </v-stage>
</template>

<script>
export default {
  data() {
    return {
      configKonva: {
        width: 1000,
        height: 800
      },
      rect: {
        name: "r0",
        x: 850,
        y: 50,
        width: 80,
        height: 30,
        stroke: 'black',
        fill: 'white',
        strokeWidth: 2,
        draggable: true
      },
      topLine: {
        points: [100, 40, 660, 40],
        stroke: "black",
        strokeWidth: "round",
        lineCap: "round",
        lineJoin: "round"
      },
      middleLine: {
        points: [380, 40, 380, 220],
        stroke: "black",
        strokeWidth: "round",
        lineCap: "round",
        lineJoin: "round"
      },
      bottomLine: {
        points: [100, 220, 660, 220],
        stroke: "black",
        strokeWidth: "round",
        lineCap: "round",
        lineJoin: "round"
      },
      snapTopLeft: {
        x: 200,
        y: 20,
        width: 100,
        height: 40,
        stroke: 'gray',
        fill: 'white',
        strokeWidth: 1
      },
      shape: null
    };
  },
  methods: {
    handle: function() {
      console.log("dzialaaa");
    },
    dragend: function(e) {
      //console.log(this.$refs);
      e.getStage().moveTo(this.$refs.layer.getStage());
      var pos = this.$refs.stage.getStage().getPointerPosition();

      if(this.shape === this.$refs.layer.getStage().getIntersection(pos)) {
        console.log("snapped");
      }
      else {
        console.log("destroyed");
        //e.getStage().destroy();
      }
    },
    drag: function(e) {
      e.getStage().stopDrag();
      var clone = e.getStage().clone({
          name: "r1",
          x: 840,
          y: 50
      });
      console.log(clone);
      console.log(e.getStage());

      clone.off('dragstart');
      this.$refs.layer.getStage().add(clone);
      clone.moveTo(this.$refs.tmpLayer.getStage());
      this.$refs.layer.getStage().draw();
      clone.startDrag();
    },
    dragmove: function(e) {
      var pos = this.$refs.stage.getStage().getPointerPosition();
      var intersection = this.$refs.layer.getStage().getIntersection(pos);
      if(intersection !== null) {
        intersection.fill("green");
        this.shape = intersection;
      }
      else if (this.shape !== null && intersection === null) {
        this.shape.fill("red");
      }
      this.$refs.layer.getStage().draw();
      //console.log(this.$refs.stage.getStage().getPointerPosition());
      //console.log(this.$refs.rekt.getStage().parent.parent.getPointerPosition());
      //console.log(stage.getPointerPosition);
    },
    referee: function(e) {
      console.log(e);
    }
  }
}
</script>
