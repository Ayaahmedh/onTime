<template>
    <div class="box">
        <div class="box-header">
            <h3 class="box-title">Invoice</h3>
        </div>
        <!-- /.box-header -->
        <div class="box-body">
            <form v-on:submit.prevent="sub" action="#" method="post">
                <table class="table table-bordered table-striped">
                    <thead>
                    <tr>
                        <th>Name</th>
                        <th>Price</th>
                        <th></th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(row, index) in rows"
                        v-bind:row="row">
                        <td><input type="text" v-model="row.name"/>
                        </td>
                        <td><p><input type="number" step="0.01" v-model="row.price"/></p></td>
                        <td><a @click="removeRow(index)" class="btn btn-danger">x</a></td>
                    </tr>
                    </tbody>
                    <tfoot>
                    <tr>
                        <th>Name</th>
                        <th>Price</th>
                        <th></th>
                    </tr>
                    </tfoot>
                </table>
                <button type="button" class="button btn-primary" @click="addRow">Add row</button>
                <button type="submit">Save</button>
            </form>

        </div>
        <!-- /.box-body -->
        <!-- /.box-header -->
        <div class="box-body" v-show="isShowing" >
            <table class="table table-bordered table-striped">
                <thead>
                <tr>
                    <th>Name</th>
                    <th>Unit Price</th>

                </tr>
                </thead>
                <tbody>
                <tr v-for="(row, index) in data">
                    <td>{{row.name}}
                    </td>
                    <td>{{row.price}}</td>
                </tr>
                </tbody>
                <tfoot>
                <tr>
                    <th>Name</th>
                    <th>Unit Price</th>
                </tr>
                </tfoot>
            </table>
            <div style="float: right;margin-right: 110px;">
                <p>All total</p>
                <p>{{total}}</p>
            </div>
        </div>

        <!-- /.box-body -->
    </div>
</template>
<script>

    export default {
        data: function () {
            return {
                data: [],
                rows: [{
                    name: "",
                    price: ""
                }],
                total: 0,
                isShowing:false
            }
        },
        methods: {
            addRow: function () {
                this.rows.push({
                    name: "",
                    price: ""
                });
            },
            removeRow: function (index) {
                this.rows.splice(index);
            },
            sub: function (event) {
                if ($.isEmptyObject(this.rows)) {
                    event.preventDefault();
                } else {
                    axios.post('/onTime/public/orders', {rows: this.rows}).then((res) => {
                        this.removeRow();
                        this.addRow();
                        this.data = res.data;
                        this.totalSum();
                        this.isShowing=true;

                    });
                }
            },
            totalSum: function () {
                this.total=0;
                this.data.forEach((item) => {
                    this.total = this.total + (1 * item.price);
                });
                this.total.toFixed(2);
            },
        }

    }
</script>
