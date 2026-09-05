<script setup lang="ts">
const periods = ['今日', '近7天', '近30天']
const activePeriod = ref('今日')
const orderFilter = ref('全部')

const metrics = [
  { label: '销售额', value: '¥ 8,426.50', change: '+12.8%', icon: 'i-lucide-banknote', tone: 'green' },
  { label: '支付订单', value: '126', change: '+8.2%', icon: 'i-lucide-shopping-bag', tone: 'blue' },
  { label: '访客数', value: '2,840', change: '+15.3%', icon: 'i-lucide-mouse-pointer-click', tone: 'amber' },
  { label: '支付转化率', value: '4.44%', change: '-0.6%', icon: 'i-lucide-percent', tone: 'rose', down: true }
]
const times = ['00:00', '04:00', '08:00', '12:00', '16:00', '20:00', '24:00']
const orders = [
  { id: '#QM20240918032', customer: '陈语安', initials: '陈', product: '原木桌面收纳盒', amount: '¥ 268.00', status: '待发货', statusClass: 'warning', time: '10分钟前' },
  { id: '#QM20240918031', customer: '周景', initials: '周', product: '手作陶瓷马克杯', amount: '¥ 159.00', status: '已发货', statusClass: 'info', time: '28分钟前' },
  { id: '#QM20240918030', customer: '许诺', initials: '许', product: '亚麻餐垫 · 暖灰', amount: '¥ 96.00', status: '已完成', statusClass: 'success', time: '42分钟前' },
  { id: '#QM20240918029', customer: '唐可', initials: '唐', product: '胡桃木托盘', amount: '¥ 328.00', status: '退款中', statusClass: 'error', time: '1小时前' }
]
const inventory = ref([
  { name: '手作陶瓷马克杯', sku: 'CM-001', stock: 3, color: '#d97757' },
  { name: '亚麻抱枕套 · 米白', sku: 'LC-012', stock: 5, color: '#8c9a7a' },
  { name: '黄铜烛台', sku: 'BC-008', stock: 7, color: '#c29b55' }
])
const tasks = ref([
  { label: '处理 8 笔待发货订单', done: false, icon: 'i-lucide-package-check' },
  { label: '回复 3 条客户咨询', done: false, icon: 'i-lucide-message-circle' },
  { label: '确认本周营销活动', done: true, icon: 'i-lucide-calendar-check' }
])
const filteredOrders = computed(() => orderFilter.value === '全部' ? orders : orders.filter(order => order.status === orderFilter.value))
const replenish = (item: typeof inventory.value[number]) => { item.stock += 20 }
</script>

<template>
  <div class="dashboard">
    <section class="page-heading">
      <div>
        <p class="eyebrow">2026年9月5日 · 星期六</p>
        <h1>早上好，林小满</h1>
        <p class="heading-copy">今天已有 126 笔订单，生意正在稳步增长。</p>
      </div>
      <div class="period-control" aria-label="数据时间范围">
        <button v-for="period in periods" :key="period" :class="{ active: activePeriod === period }" @click="activePeriod = period">{{ period }}</button>
      </div>
    </section>

    <section class="metric-grid" aria-label="核心经营指标">
      <article v-for="metric in metrics" :key="metric.label" class="metric-card">
        <div :class="['metric-icon', `metric-icon--${metric.tone}`]"><UIcon :name="metric.icon" class="size-5" /></div>
        <div class="metric-label">{{ metric.label }}<UTooltip text="统计口径为已支付订单"><UIcon name="i-lucide-circle-help" class="size-3.5" /></UTooltip></div>
        <strong>{{ metric.value }}</strong>
        <div class="metric-change">
          <span :class="metric.down ? 'trend-down' : 'trend-up'"><UIcon :name="metric.down ? 'i-lucide-trending-down' : 'i-lucide-trending-up'" class="size-3.5" />{{ metric.change }}</span>
          <span>较昨日</span>
        </div>
      </article>
    </section>

    <section class="dashboard-grid dashboard-grid--top">
      <article class="panel sales-panel">
        <div class="panel-header">
          <div><h2>销售趋势</h2><p>今日各时段支付金额</p></div>
          <div class="chart-legend"><span /> 今日销售额</div>
        </div>
        <div class="chart-summary"><strong>¥ 8,426.50</strong><span><UIcon name="i-lucide-arrow-up-right" /> 12.8%</span></div>
        <div class="chart" aria-label="今日销售额折线图">
          <div class="y-labels"><span>¥10k</span><span>¥7.5k</span><span>¥5k</span><span>¥2.5k</span><span>¥0</span></div>
          <div class="chart-plot">
            <div v-for="i in 5" :key="i" class="grid-line" />
            <svg viewBox="0 0 600 180" preserveAspectRatio="none" role="img" aria-label="销售额持续上升">
              <defs><linearGradient id="sales-fill" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#159a6e" stop-opacity="0.22" /><stop offset="100%" stop-color="#159a6e" stop-opacity="0" /></linearGradient></defs>
              <path d="M0 150 C55 156 70 142 100 146 S170 130 200 122 S265 92 300 96 S365 75 400 70 S465 28 500 38 S555 58 600 52 L600 180 L0 180 Z" fill="url(#sales-fill)" />
              <path d="M0 150 C55 156 70 142 100 146 S170 130 200 122 S265 92 300 96 S365 75 400 70 S465 28 500 38 S555 58 600 52" fill="none" stroke="#159a6e" stroke-width="3" vector-effect="non-scaling-stroke" />
              <circle cx="500" cy="38" r="5" fill="#fff" stroke="#159a6e" stroke-width="3" vector-effect="non-scaling-stroke" />
            </svg>
            <div class="x-labels"><span v-for="time in times" :key="time">{{ time }}</span></div>
          </div>
        </div>
      </article>

      <article class="panel channel-panel">
        <div class="panel-header"><div><h2>流量来源</h2><p>访客渠道分布</p></div><UButton icon="i-lucide-ellipsis" color="neutral" variant="ghost" aria-label="更多流量数据" /></div>
        <div class="donut-wrap"><div class="donut"><div class="donut-center"><strong>2,840</strong><span>总访客</span></div></div></div>
        <div class="channel-list">
          <div><span class="channel-dot channel-dot--green" />自然搜索 <strong>42%</strong></div>
          <div><span class="channel-dot channel-dot--blue" />社交媒体 <strong>31%</strong></div>
          <div><span class="channel-dot channel-dot--yellow" />直接访问 <strong>18%</strong></div>
          <div><span class="channel-dot channel-dot--gray" />其他渠道 <strong>9%</strong></div>
        </div>
      </article>
    </section>

    <section class="dashboard-grid dashboard-grid--middle">
      <article class="panel orders-panel">
        <div class="panel-header panel-header--orders">
          <div><h2>最新订单</h2><p>实时查看店铺订单动态</p></div>
          <div class="order-actions">
            <select v-model="orderFilter" aria-label="筛选订单状态"><option>全部</option><option>待发货</option><option>已发货</option><option>已完成</option><option>退款中</option></select>
            <UButton label="查看全部" trailing-icon="i-lucide-arrow-right" color="neutral" variant="ghost" />
          </div>
        </div>
        <div class="order-table-wrap">
          <table class="order-table">
            <thead><tr><th>订单 / 客户</th><th>商品</th><th>金额</th><th>状态</th><th>时间</th></tr></thead>
            <tbody><tr v-for="order in filteredOrders" :key="order.id">
              <td><div class="customer-cell"><span class="customer-avatar">{{ order.initials }}</span><div><strong>{{ order.customer }}</strong><span>{{ order.id }}</span></div></div></td>
              <td>{{ order.product }}</td><td><strong>{{ order.amount }}</strong></td><td><span :class="['status-tag', `status-tag--${order.statusClass}`]">{{ order.status }}</span></td><td>{{ order.time }}</td>
            </tr></tbody>
          </table>
        </div>
      </article>

      <article class="panel tasks-panel">
        <div class="panel-header"><div><h2>今日待办</h2><p>{{ tasks.filter(task => !task.done).length }} 项尚未完成</p></div><UButton icon="i-lucide-plus" color="neutral" variant="ghost" aria-label="添加待办" /></div>
        <div class="task-list">
          <label v-for="task in tasks" :key="task.label" :class="['task-item', { done: task.done }]">
            <UCheckbox v-model="task.done" /><span class="task-symbol"><UIcon :name="task.icon" /></span><span>{{ task.label }}</span>
          </label>
        </div>
        <button class="schedule-link">查看经营日程 <UIcon name="i-lucide-arrow-right" /></button>
      </article>
    </section>

    <section class="panel inventory-panel">
      <div class="panel-header"><div><h2>库存预警</h2><p>以下商品库存低于安全库存，请及时补货</p></div><UButton label="库存管理" trailing-icon="i-lucide-arrow-right" color="neutral" variant="ghost" /></div>
      <div class="inventory-grid">
        <div v-for="item in inventory" :key="item.sku" class="inventory-item">
          <div class="product-thumb" :style="{ backgroundColor: item.color }"><UIcon name="i-lucide-package" /></div>
          <div class="product-copy"><strong>{{ item.name }}</strong><span>SKU: {{ item.sku }}</span></div>
          <div class="stock-count"><span>剩余</span><strong>{{ item.stock }} 件</strong></div>
          <UButton label="补货" color="neutral" variant="outline" size="sm" @click="replenish(item)" />
        </div>
      </div>
    </section>
  </div>
</template>
