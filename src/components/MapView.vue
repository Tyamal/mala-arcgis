<template>
  <div>
    <h2>Map View</h2>
    <input type="file" @change="onFileChange" />
    <canvas id="chart" width="400" height="200"></canvas>
    <div id="viewDiv" style="height: 500px;"></div>
  </div>
</template>

<script>
import MapView from '@arcgis/core/views/MapView';
import WebMap from '@arcgis/core/WebMap';
import Chart from 'chart.js/auto';

export default {
  name: 'MapView',
  data() {
    return {
      view: null,
      chart: null,
    };
  },
  mounted() {
    const webMap = new WebMap({
      portalItem: {
        id: 'your-webmap-id' // Replace with your WebMap ID
      }
    });

    this.view = new MapView({
      container: 'viewDiv',
      map: webMap,
      zoom: 2,
      center: [0, 0]
    });
  },
  methods: {
    onFileChange(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        const data = JSON.parse(e.target.result);
        this.updateChart(data);
        this.addDataToMap(data);
      };
      reader.readAsText(file);
    },
    updateChart(data) {
      const labels = data.features.map(feature => feature.properties.name); // Adjust based on your data structure
      const values = data.features.map(feature => feature.properties.value); // Adjust based on your data structure

      if (this.chart) {
        this.chart.destroy();
      }

      const ctx = document.getElementById('chart').getContext('2d');
      this.chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: 'Data Values',
            data: values,
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    },
    addDataToMap(data) {
      // Add your logic to display data on the
