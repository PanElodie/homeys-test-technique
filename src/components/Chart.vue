<script>
    import {
        Bar
    } from 'vue-chartjs'

    export default {
        name: 'Chart',
        extends: Bar,
        data() {
            return {
                data: {
                    labels: ['January', 'February']
                },
                options: {
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true
                            },
                            gridLines: {
                                display: true
                            }
                        }],
                        xAxes: [{
                            gridLines: {
                                display: false
                            }
                        }]
                    },
                    legend: {
                        display: true
                    },
                    responsive: true,
                    maintainAspectRatio: false
                }
            }
        },
        mounted() {
            this.renderChart(this.data, this.options)
        }
    }
</script>

<style>
</style>