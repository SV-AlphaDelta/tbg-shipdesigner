<template>

  <tr class="subsystem-frame">
	<td class="name-column" colspan="2">{{se_subsystem.name}}</td>

	<td class="part-column">
	  <select v-model="sub_frame" class="part-column-select" v-bind:class="part_column_select_computed">
		<option v-for="sub_frame_value in se_subsystem.valid_frames">{{sub_frame_value['Name']}}</option>
		<option v-if="!is_valid_frame">{{sub_frame}}</option>
	  </select>
	</td>

	<template v-for="name in stats.names">
	  <td class="monospace-cell">{{stats_multiplier_pretty}}</td>
	</template>

	<td class="weight-internal-column monospace-cell" colspan="2">{{se_subsystem.weight_cap.toFixed(2)}}</td>

	<td class="br-column monospace-cell">{{se_subsystem.cost_BR_frame.toFixed(2)}}</td>
	<td class="sr-column monospace-cell">{{se_subsystem.cost_SR_mult.toFixed(2)}}x</td>

	<td class="power-cost-column monospace-cell"></td>
	<td class="power-gen-column monospace-cell"></td>

	<template v-for="name in crew.names">
	  <StatlineCell :stats="crew_mult_pretty" :name="name" :ispretty="false"></StatlineCell>
	</template>

	<td class="build-time-column monospace-cell">{{build_time}}</td>
  </tr>


</template>


<script>

import ShipEngine from '../lib/shipengine.js';

import StatlineCell from './statline-cell.vue';

import { frac } from './ui-functions.js';

export default {
	name: 'SubsystemFrame',
	components: {
		StatlineCell,
	},
	props: {
		se_db: Object,
		se_subsystem: Object,
	},
	computed: {
		part_column_select_computed () {
			return {
				['has-error']: !this.is_valid_frame,
			};
		},
		is_valid_frame () {
			return this.valid_frames
				.map((frame) => frame['Name'])
				.includes(this.sub_frame);
		},
		valid_frames () {
			return this.se_subsystem.valid_frames;
		},
		se_components () {
			return this.se_subsystem.components;
		},
		stats() {
			return this.se_subsystem.stats;
		},
		stats_multiplier_pretty() {
			return this.se_subsystem.stats_multiplier.toFixed(2) + 'x';
		},
		crew() {
			return this.se_subsystem.cost_crew;
		},
		crew_mult_pretty() {
			return this.se_subsystem.cost_crew_frame_mult.apply((val) => val.toFixed(2) + 'x');
		},
		build_time () {
			return frac(this.se_subsystem.build_time, 12);
		},
		sub_frame: {
			get () {
				return this.se_subsystem.sub_frame;
			},
			set (value) {
				this.se_subsystem.sub_frame = value;
			},
		},
	},
	methods: {
	},
}
</script>


<style>

</style>


<style scoped>
.subsystem-frame {
	background: #ccc;
}

.has-error {
	background: #faa;
}
</style>
