<script>
import { Pie, mixins } from "vue-chartjs";
const { reactiveProp } = mixins;
export default {
  extends: Pie,
  mixins: [reactiveProp],
  mounted() {
    // 下記データでグラフをレンダリング
    this.renderChart(this.chartData);
  }
};
</script>