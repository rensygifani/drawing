<template>
  <div>
    <canvas ref="canvas" width="280" height="280" @mousedown="startDrawing" @mousemove="draw" @mouseup="stopDrawing"></canvas>
    <button class="btn btn-success" @click="clearCanvas">Clear</button>
    <button class="btn btn-primary" @click="predictNumber">Predict Number</button>

    <!-- Display predicted digit -->
    <p id="predict">Predicted Digit: {{ predictedDigit }}</p>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      isDrawing: false,
      x: 0,
      y: 0,
      predictedDigit: "5",
    };
  },
  mounted() {
    this.canvas = this.$refs.canvas;
    this.ctx = this.canvas.getContext('2d');
  },
  methods: {
    startDrawing(event) {
      this.isDrawing = true;
      this.x = event.offsetX;
      this.y = event.offsetY;
    },
    draw(event) {
      if (!this.isDrawing) return;
      this.ctx.beginPath();
      this.ctx.moveTo(this.x, this.y);
      this.ctx.lineTo(event.offsetX, event.offsetY);
      this.ctx.stroke();
      this.x = event.offsetX;
      this.y = event.offsetY;
    },
    stopDrawing() {
      this.isDrawing = false;
    },
    clearCanvas() {
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    },
    async predictNumber() {
      // Convert canvas to base64 data URL
      const image = this.canvas.toDataURL();
      const base64String = image.split(',')[1];

      // Prepare request data
      const requestData = {
        image: base64String,
        // image : "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PPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPOPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPOHPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPJPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPMPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPNPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPDDDDFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPPPPPFPPPPPPPFPPPPPPPPPPPPPPPPPPPPPPBDDDHPPPPPPPDDDDNPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPFPPPPPPPPPPPPPPPPPPOMMMMMMMMMMMMMMMNPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPPP//EABQRAQAAAAAAAAAAAAAAAAAAAKD/2gAIAQMBAT8QRP8A/8QAFBEBAAAAAAAAAAAAAAAAAAAAoP/aAAgBAgEBPxBE/wD/xAAiEAEAAgMAAgIDAQEAAAAAAAABEaEhUGAxQQAwQFFxcGH/2gAIAQEAAT8Q/wBphd3k9BJAkiP8T8CFChQoUKFChQoUKFChNJuFKgoRiUJ/6bTzBfAyEoMwsvA4gR8kjaS5SSORmckhAkvv0000000000000008+JyKcyz4wTGNmcD54qVHAAKr8h3WrXw8E5AL5LIJwbJUFoUUBJMY2FkCvxTNN58pD9qGAM4Ag4NlMsyiUpUk4VwirQSMDqYGAEpySdGYIWvBMsjlKSpIBWAcBPw1NhsinPYUFAKThDKRBExmA5SQQrMbCcw7ACAzEBSUglV98GymWZRKUqScK4RVoJHoQEWQLn5MIqhyuDZZHKUlSQCsA4CfkCeAhGRYrCYwSEhHBspEETGYDlJBCsxsJzDsAIDMQFJSCVX3wbKZZlEpSpJwrhFWgkehARZAufkwiqHK4NlP5qGB4MGVVACVUAVPkeW6kQmCGdmAAyPBsgPTL0wsAnkZQkwkuYdgBAZiApKQSq++EZLAsWZpB8grp7h+jhWVZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuymGmaqABCCmH37x9BYsWLFixYDluyuJSKIk9es8KyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrOF2VZwuyrN7snuM7NUpLEjEh9FKlSpUqVIZvGgUgCkwkwv4Dv6SwmMCDyPfifopUqVKlSpRSdHCQGYTCURKTBKaSs0HeaXrNB3ml6zQd5pes0HeaXrNB3ml6zQd5pes0HeaXrNB3ml6zQd5pes0HeaXrNB3ml6zQd5pes0HeaXrNB3ml6zQd5pfKa390QJiSY8SaCFChQoUKFChQoUKFChQoUKFChQoUKFChQoUKFhNb+6IMTDE+YdL5PKVqSABKuAjQaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaRvZhD8ScjyMYQSEH/AGL/2Q==",
      };
      

      // Set headers for the Axios POST request
      const config = {
        headers: {
          'Content-Type': 'application/json',
        },
      };

      try {
        // Send POST request with headers to a specified endpoint
        const response = await axios.post('http://127.0.0.1:5000/detect_number', requestData, config);

        const keyValueArray = Object.entries(response.data).map(([key, value]) => ({
          key,
          value,
        }));

        // Log each key-value pair
        keyValueArray.forEach(item => {
          console.log(`Key: ${item.key}, Value: ${item.value}`);
          this.predictedDigit = item.value;
        });

        // this.predictedDigit = response.data.prediction;

        // Log the response from the backend
        // console.log( response.data);
        
      } catch (error) {
        // Log any errors
        console.error('Error predicting number:', error);
      }
    },
  },
};
</script>

<style scoped>
canvas {
  border: 1px solid #000;
}
</style>
