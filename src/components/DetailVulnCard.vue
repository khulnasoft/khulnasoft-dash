<template>
	<v-card
	tile
	elevation="5"
	class="rounded-card"
	height="auto"
	>
  <div class="vld-parent">
    <loading :active.sync="riskTableLoading" 
    :can-cancel="false"
    :is-full-page="false"></loading>
  </div>
  <v-card-title>
   <span class="title font-weight-light ">
    {{title.toUpperCase()}}
    <v-chip
    class="ma-2"
    text-color="white"
    label
    >

    {{risks.length}}
  </v-chip>
</span>
<v-spacer/>
  <v-text-field
  v-model="search"
  append-icon="mdi-magnify"
  label="Search"
  single-line
  hide-details
  ></v-text-field>
<v-spacer/>
<span class="title font-weight-thin ">
				<!--v-chip
				class="ma-2"
				text-color="white"
				label
				>
					
					{{noPatchCount}}
					<v-icon x-small>
            mdi-shield-remove-outline
          </v-icon>
        </v-chip-->
        <v-chip
        class="ma-2"
        color="info darken-4"
        text-color="white"
        label
        >

        {{patchAvailableCount}}
        <v-icon x-small>
          mdi-shield-plus-outline
        </v-icon>
      </v-chip>
      <v-chip
      class="ma-2"
      color="info darken-2"
      text-color="white"
      label
      >

      {{patchedAuditCount}}
      <v-icon x-small>
        mdi-shield-sync-outline
      </v-icon>
    </v-chip>
    <v-chip
    class="ma-2"
    color="info darken-1"
    text-color="white"
    label
    >

    {{patchedEnforceCount}}
    <v-icon x-small>
      mdi-shield-check-outline
    </v-icon>
  </v-chip>
</span>
</v-card-title>
<v-data-table
:headers="headers"
:items="risks"
:search="search"
item-key="id"
sort-by="v_patch_status"
group-by="image_name"
class="elevation-1"
show-group-by
>
<template v-slot:item.nvd_url="{ item }">
 <v-btn 
 :href="item.nvd_url"
 text 
 small
 dense
 >nvd</v-btn>
 <!--v-chip :color="getColor(item.calories)" dark>{{ item.calories }}</v-chip-->
</template>
<template v-slot:item.v_patch_status="{ item }">
 <v-btn 
 v-if="item.v_patch_status === 'patch_available'"
 text 
 small 
 color="info"
 >
 <v-icon x-small>
   mdi-shield-plus-outline
 </v-icon>
</v-btn>
<v-btn 
v-if="item.v_patch_status === 'no_patch'"
text 
small
disabled
>
<v-icon x-small>
 mdi-shield-remove-outline
</v-icon>
</v-btn>
<v-btn 
v-if="item.v_patch_status === 'patched_audit'"
text 
small
>
<v-icon x-small>
 mdi-shield-sync-outline
</v-icon>
</v-btn>
<v-btn 
v-if="item.v_patch_status === 'patched_enforce'"
text 
small
>
<v-icon x-small>
 mdi-shield-check-outline
</v-icon>
</v-btn>
<!--v-chip :color="getColor(item.calories)" dark>{{ item.calories }}</v-chip-->
</template>
</v-data-table>
</v-card>
</template>
<script>
	import Loading from 'vue-loading-overlay'
	import 'vue-loading-overlay/dist/vue-loading.css'

	export default {
		data () {
			return {
				headers: [
       {
        text: 'Vulnerabilities',
        align: 'start',
        value: 'name',
        groupable: false,
        sortable: false
      },
      {
        text: 'Image',
        align: 'start',
        value: 'image_name'
      },
      { 
        text: 'Issue Description',
        value: 'description',
        align: 'left',
        groupable: false,
        sortable: false
      },
      { 
        text: 'Vshield',
        value: 'v_patch_status',
        align: 'right'
      },
      { 
        text: 'Remediation',
        value: 'solution',
        align: 'left',
        groupable: false,
        sortable: false
      },
      { 
        text: 'Link to Khulnasoft',
        value: 'nvd_url',
        align: 'left',
        groupable: false,
        sortable: false
      }
      ],
      search: ""
    }
  },
  props: ['risks', 'title'],
  components: {
   Loading
 },
 computed: {
   riskTableLoading () {
    return false
  },
  noPatchCount () {
    return this.risks.filter(function(obj) {
     return obj.v_patch_status === 'no_patch'
   }).length
  },
  patchAvailableCount () {
    return this.risks.filter(function(obj) {
     return obj.v_patch_status === 'patch_available'
   }).length
  },
  patchedAuditCount () {
    return this.risks.filter(function(obj) {
     return obj.v_patch_status === 'patched_audit'
   }).length
  },
  patchedEnforceCount () {
    return this.risks.filter(function(obj) {
     return obj.v_patch_status === 'patched_enforce'
   }).length
  }
}
}
</script>