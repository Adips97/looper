<template>
<div>
    <div class="d-flex flex-column-fluid">
        <!--begin::Container-->
        <div class="container-fluid">
            <div class="row">
                <div class="col-12">
                    <div class="row">
                        <div class="col-lg-12 col-xl-12">
                            <div class="card card-custom gutter-b bg-transparent shadow-none border-0">
                                <div class="card-header align-items-center  border-bottom-dark px-0">
                                    <div class="card-title mb-0">
                                        <h3 class="card-label mb-0 font-weight-bold text-body">
                                            Product Variation
                                        </h3>
                                    </div>
                                    <div class="icons d-flex">
                                        <button class="btn ml-2 p-0 kt_notes_panel_toggle" data-toggle="tooltip" title="" data-placement="right" data-original-title="Check out more demos" v-if="$parent.permissions.includes('product-variation-manage')">
                                            <span class="bg-secondary h-30px font-size-h5 w-30px d-flex align-items-center justify-content-center  rounded-circle shadow-sm " v-on:click="
                                                        display_form = !display_form
                                                    ">
                                                <svg width="25px" height="25px" viewBox="0 0 16 16" class="bi bi-plus white" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                                                    <path fill-rule="evenodd" d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"></path>
                                                </svg>
                                            </span>
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-12 ">
                            <div class="card card-custom gutter-b bg-white border-0">
                                <div class="card-body">
                                    <div>
                                        <div class=" table-responsive" id="printableTable">
                                        
                                            <div id="productvariationTable_wrapper" class="dataTables_wrapper no-footer">

                                            <div class="dataTables_length" id="productvariationTable_length"><label>Show 
                                            <select name="productvariationTable_length"  class="" v-model="limit" v-on:change="fetchvariations()">
                                            <option value="10">10</option>
                                            <option value="25">25</option>
                                            <option value="50">50</option>
                                            <option value="100">100</option>
                                            <option value="200">200</option>
                                            <option value="500">500</option>
                                            <option value="1000">1000</option>
                                            </select> entries</label></div>

                                            <div id="productvariationTable_filter" class="dataTables_filter">
                                                <label>Search:<input type="text" class="" placeholder=""  v-model="searchParameter" @keyup="fetchvariations()"></label>
                                                <button v-if="this.searchParameter != ''" @click="clearSearch">clear</button>
                                            </div>
                                                <table id="productvariationTable" class="display dataTable no-footer" role="grid">
                                                    <thead class="text-body">
                                                        <tr role="row">
                                                            <th class="sorting" tabindex="0"  rowspan="1" colspan="1" aria-sort="ascending" aria-label="ID: activate to sort column descending" style="width: 31.25px;" @click="sorting('id')" :class="(this.$data.sortType == 'asc' || this.$data.sortType == 'ASC') && this.$data.sortBy == 'id'  ? 'sorting_asc' : (this.$data.sortType == 'desc' || this.$data.sortType == 'DESC') && this.$data.sortBy == 'id' ? 'sorting_desc' : 'sorting'">
                                                                ID
                                                            </th>
                                                            <th class="sorting" tabindex="0"  rowspan="1" colspan="1" aria-label="variation: activate to sort column ascending" style="width: 95.5288px;" @click="sorting('name')" :class="(this.$data.sortType == 'asc' || this.$data.sortType == 'ASC') && this.$data.sortBy == 'name'  ? 'sorting_asc' : (this.$data.sortType == 'desc' || this.$data.sortType == 'DESC') && this.$data.sortBy == 'name' ? 'sorting_desc' : 'sorting'">
                                                            Name
                                                            </th>
                                                            <th class="sorting" tabindex="0"  rowspan="1" colspan="1" aria-label="variation: activate to sort column ascending" style="width: 95.5288px;" @click="sorting('attribute')" :class="(this.$data.sortType == 'asc' || this.$data.sortType == 'ASC') && this.$data.sortBy == 'attribute'  ? 'sorting_asc' : (this.$data.sortType == 'desc' || this.$data.sortType == 'DESC') && this.$data.sortBy == 'attribute' ? 'sorting_desc' : 'sorting'">
                                                                Attribute
                                                            </th>
                                                            <th v-if="$parent.permissions.includes('product-variation-manage')" class="no-sort sorting_disabled" rowspan="1" colspan="1" aria-label="Action" style="width: 53.1891px;">
                                                                Action
                                                            </th>
                                                        </tr>
                                                    </thead>
                                                    <tbody class="kt-table-tbody text-dark">
                                                        <tr class="kt-table-row kt-table-row-level-0 odd" role="row" v-for="variation in variations" v-bind:key="variation.id">
                                                            <td class="sorting_1">
                                                                {{variation.id}}
                                                            </td>
                                                            <td>
                                                            {{ variation.detail == null ? '' : (variation.detail[0] ? variation.detail[0].name : '') }}
                                                            </td>
                                                            <td>
                                                                {{ variation.attribute == null ? '' : (variation.attribute.detail[0] ? variation.attribute.detail[0].name : '') }}
                                                            </td>
                                                            <td v-if="$parent.permissions.includes('product-variation-manage')">
                                                            <a href="javascript:void(0)" class=" click-edit" id="click-edit1" data-toggle="tooltip" title="" data-placement="right" data-original-title="Check out more demos" @click="editvariation(variation)"><i class="fa fa-edit"></i></a>
                                                                        <a class="" href="#" @click="deletevariation(variation.id)"><i class="fa fa-trash"></i></a>
                                                            </td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                                <ul class="pagination pagination-sm m-0 float-right">
                                                    <li v-bind:class="[{disabled: !pagination.prev_page_url}]"><button class="page-link" href="#" @click="fetchvariations(pagination.prev_page_url)">Previous</button></li>

                                                    <li class="disabled"><button class="page-link text-dark" href="#">Page {{ pagination.current_page }} of {{ pagination.last_page }}</button></li>

                                                    <li v-bind:class="[{disabled: !pagination.next_page_url}]" class="page-item"><button class="page-link" href="#" @click="fetchvariations(pagination.next_page_url)">Next</button></li>
                                                </ul>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="offcanvas offcanvas-right kt-color-panel p-5 kt_notes_panel" v-if="display_form" :class="display_form ? 'offcanvas-on' : ''">
        <div class="offcanvas-header d-flex align-items-center justify-content-between pb-3">
            <h4 class="font-size-h4 font-weight-bold m-0">Add variation</h4>
            <a href="#" class="btn btn-sm btn-icon btn-light btn-hover-primary kt_notes_panel_close" v-on:click="clearForm()">
                <svg width="20px" height="20px" viewBox="0 0 16 16" class="bi bi-x" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"></path>
                </svg>
            </a>
        </div>
        <form id="myform">
            <div class="row">
                <div class="col-12">
                    <div class="tabslang">
                    <div v-for="language in languages" class="tablang" :class="language.id == selectedLanguage ?'active':''" @click="setSelectedLanguage(language.id)">
                        {{ language.language_name }}
                    </div>
                    </div>
                   <div class="form-group " v-for="(language,index) in languages" v-if="language.id == selectedLanguage">
                        <label class="text-dark">Name ( {{ language.language_name }} ) </label>
                        <input type="text" :name="'name'+index" v-model="variation.name[index]" class="form-control" />
                        <small class="form-text text-danger" v-if="errors.has('name')" v-text="errors.get('name')"></small>
                    </div>
                    <div class="form-group">
                        <label>Attribute</label>
                        <fieldset class="form-group mb-3">
                            <select class="js-example-basic-single js-states form-control bg-transparent" v-model='variation.attribute_id' >
                                <option value="">Select Attribute</option>
                                <option 
                                v-for='attribute in attributes' :value='attribute.attribute_id'
                                v-bind:selected="variation.attribute_id == attribute.attribute_id"
                                v-bind:key="attribute.attribute_id">{{ attribute.detail == null ? '' : (attribute.detail[0] ? attribute.detail[0].name : '') }}</option>
                            </select>
                            <small class="form-text text-danger" v-if="errors.has('attribute_id')" v-text="errors.get('attribute_id')"></small>
                        </fieldset>
                    </div>
                </div>

                
            </div>
            <button type="button" @click="addUpdatevariation()" class="btn btn-primary">Submit</button>
        </form>
    </div>
</div>
</template>

<script>
import ErrorHandling from "./../../ErrorHandling";
export default {
    data() {
        return {
            display_form: 0,
            variation: {
                id: "",
                name:[],
                attribute_id:'',
                language_id:[]
            },
            searchParameter: '',
            sortBy: 'id',
            sortType: 'ASC',
            limit: 10,
            error_message: '',
            edit: false,
            actions: false,
            pagination: {},
            request_method: "",
            countrySelected: "",
            stateSelected: "",
            variations: [],
            attributes: [],
            languages: [],
            token: [],
            selectedLanguage:'',
            errors: new ErrorHandling(),
            csrf: document.querySelector('meta[name="csrf-token"]').getAttribute("content")
        };
    },

    methods: {
        fetchLanguages() {
            this.$parent.loading = true;
            var token = localStorage.getItem('token');
            const config = {
                headers: {
                    Authorization: `Bearer ${token}`
                }
            };
            axios.get('/api/admin/language?limit=500', config)
                .then(res => {
                    if (res.data.status == "Success") {
                        this.languages = res.data.data;
                        for(var i = 0 ; i < res.data.data.length; i++){
                            this.variation.language_id.push(res.data.data[i].id);
                            if(res.data.data[i].is_default){
                                this.selectedLanguage = res.data.data[i].id;   
                            }
                        }
                    }
                })
                .finally(() => (this.$parent.loading = false));
        },
        fetchattributes(page_url) {
            let vm = this;
            page_url = page_url || "/api/admin/attribute?getDetail=1";
            axios.get(page_url, this.token).then(res => {
                this.attributes = res.data.data;
            });
        },
        fetchvariations(page_url) {
            this.$parent.loading = true;
            let vm = this;
            page_url = page_url || "/api/admin/variation";
            var arr = page_url.split('?');
            
            if (arr.length > 1) {
                page_url += '&limit='+this.limit;
            }
            else{
                page_url += '?limit='+this.limit;
            }
            if(this.searchParameter != null){
                page_url += '&searchParameter='+this.searchParameter;
            }
            page_url += '&sortBy='+this.sortBy+'&sortType='+this.sortType+'&getDetail=1&getAttribute=1';
            var responseData = {};

            axios.get(page_url, this.token).then(res => {
                this.variations = res.data.data;
                vm.makePagination(res.data.meta, res.data.links);
            })
            .finally(() => (this.$parent.loading = false));
        },

        makePagination(meta, links) {
            let pagination = {
                current_page: meta.current_page,
                last_page: meta.last_page,
                next_page_url: links.next,
                prev_page_url: links.prev
            };

            this.pagination = pagination;
        },
        deletevariation(id) {
            if (confirm('Are You Sure?')) {
                this.$parent.loading = true;
                axios.delete(`/api/admin/variation/${id}`,this.token)
                    .then(res => {
                        if (res.data.status == "Success") {
                            this.$toaster.success('Product Variation has been Delete successfully')
                            this.fetchvariations();
                        }

                    })
                    .catch(error => {
                        if (error.response.status == 422) {
						if(error.response.data.status == 'Error'){
                             this.$toaster.error(error.response.data.message)
						} 
					}
                    })
                    .finally(() => (this.$parent.loading = false));
            }
        },
        addUpdatevariation() {
            this.$parent.loading = true;
            var url = '/api/admin/variation';
            if (this.edit === false) {
                // Add
                this.request_method = 'post'
            } else {
                // Update
                var url = '/api/admin/variation/' + this.variation.id;
                this.request_method = 'put'
            }
            axios[this.request_method](url, this.variation, this.token)
                .then(res => {
                    if (res.data.status == "Success") {
                        this.display_form = 0;
                        this.$toaster.success('Product Variation has been updated successfully')
                        this.clearForm();
                        this.fetchvariations();
                    } else {
                        this.$toaster.error(res.data.message)
                    }
                })
                .catch(error => {
					this.error_message = '';
					this.errors = new ErrorHandling();
					if (error.response.status = 422) {
						if(error.response.data.status == 'Error'){
							this.error_message = error.response.data.message;
						}
						else{
							this.errors.record(error.response.data.errors);
						}
					}
				}).finally(() => (this.$parent.loading = false));

        },
        editvariation(variation) {
            this.edit = true;
            this.display_form = 1;
            this.errors = new ErrorHandling();
            this.variation.id = variation.id;
            this.variation.name = [];
            this.$parent.loading = true;
            axios.get(`/api/admin/variation/${variation.id}?getDetail=1&getAttribute=1`,this.token)
            .then(res => {
                if (res.data.status == "Success") {
                    res.data.data.detail.forEach(u => {
                        // this.variation.name.push(u.name);
                        this.variation.name[this.variation.language_id.indexOf(u.language.id)] = u.name;
                        
                    });
                }
               this.variation.attribute_id = res.data.data.attribute.attribute_id;
               this.variation = Object.assign({}, this.variation, { attribute_id: res.data.data.attribute.attribute_id })

            })
            .catch(err => console.log(err))
            .finally(() => (this.$parent.loading = false));
            
            
        },
        clearForm() {
            this.display_form = 0;
            this.edit = false;
            
            this.variation.id =  "";
            this.variation.name = [];
            this.variation.attribute_id = '';
            },
        sorting(sortBy){
            this.sortBy = sortBy;
            this.sortType = this.sortType == 'asc' || this.sortType == 'ASC' ? this.sortType='desc' : this.sortType = 'asc';
            this.fetchvariations();
        },
        setSelectedLanguage(selectedLanguage){
            this.selectedLanguage = selectedLanguage;
            console.log("i am clicked",selectedLanguage)
        },
        clearSearch(){
            this.searchParameter = "",
            this.fetchvariations();
        }
    },
    mounted() {
        
        var token = localStorage.getItem('token');
        this.token = {
            headers: {
                Authorization: `Bearer ${token}`
            }
        };
        this.fetchvariations();
        this.fetchLanguages();
        this.fetchattributes();
    }
};
</script>
