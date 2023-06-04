<template>
  <table>
    <thead>
      <tr>
        <th @click="sortByColumn('name')">Имя</th>
        <th @click="sortByColumn('phone')">Номер</th>
      </tr>
    </thead>
    <tbody v-for="(group, idx) in sortedChildren" :key="idx">
      <tr v-for="(child, idx) in group" :key="child.id">
        <td :style="{ paddingLeft: `${(idx + 1) * 20}px` }">{{ child.name }}</td>
        <td :style="{ paddingLeft: `${(idx + 1) * 20}px` }">{{ child.phone }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>

export default {
  name: 'UserList',
  props: {
    userData: {
      type: Array
    }
  },
  data() {
    return {
      sortColumn: 'name',
      sortOrder: 1, 
    };
  },
  computed: {
    flattenedData() {
      return this.flattenArray(this.userData)
    },
    sortedChildren() {
      const { flattenedData, sortColumn, sortOrder } = this

      const sortedData = sortColumn
        ? flattenedData.map((arr) =>
            [...arr].sort((a, b) =>
              sortColumn === 'name'
                ? a[sortColumn].localeCompare(b[sortColumn]) * sortOrder
                : (b[sortColumn] - a[sortColumn]) * sortOrder
            )
          )
        : [...flattenedData]

      return sortedData
    }
  },
  methods: {
    flattenArray(arr) {
      let result = [];
      for (const { children, ...element } of arr) {
        result.push([{ ...element }])

        if (children?.length > 0) {
          const childResult = this.flattenArray(children)
          for (const child of childResult) {
            result[result.length - 1].push(...child)
          }
        }
      }
      return result
    },
    sortByColumn(column) {
      if (this.sortColumn === column) {
        this.sortOrder = this.sortOrder === 1 ? -1 : 1;
      } else {
        this.sortColumn = column
      }
    },
  },
}
</script>

<style>
table {
  border-collapse: collapse;
  width: 50%;
}

th, td {
  padding: 8px;
  text-align: left;
  border: 1px solid black;
}

tr:hover {
  background-color: #ddd;
}
</style>