# validator-jquery
validator  for jquery
function nextToSaveStore() 
{
    $("#modal_store").validate({
        rules: {
			required: !0
        }
        , invalidHandler:function(e, r) {
            mUtil.scrollTo("modal_store", -200), swal({
                title:"", 
                text:"There are some errors in your submission. Please correct them.", 
                type:"error", 
                confirmButtonClass:"btn btn-secondary m-btn m-btn--wide", 
                onClose:function(e) {
                    console.log("on close event fired!")
                }
            }), e.preventDefault()
        }
        , submitHandler:function(e) {
        }
    });

	if($("#modal_store").valid()) {
		swal({
			title: "Are you sure?",
			text: "You won't be able to save this!",
			type: "warning",
			showCancelButton: !0,
			confirmButtonText: "Yes, Save it!",
			cancelButtonText: "No, cancel!",
			reverseButtons: !0
		}).then(function(result){
			if(result.value) {
				$.ajax({
					url: '<?php echo site_url('master/store/store_controller/action_store'); ?>',
					type: 'POST',
					data: $('#modal_store').serialize(),
					beforeSend: function() {
						$('#btn_submit').prop('disabled', true).addClass('m-loader m-loader--light m-loader--right').text('Submitting...');
					},
					success: function(result2)
					{
						swal({
							title: 'Success',
							text: ($('input[name="name_modal"]').val() != '' ? 'Store has been updated' : 'Store has been created'),
							type: 'success',
							showCancelButton: false,
							confirmButtonText: 'OK!'
							})
						.then(function(result3) {
							location.href = '<?php echo base_url();?>data-store';
						});
					},
					error:function(){
						swal("Confirmation", ($('input[name="name_modal"]').val() != '' ? 'Data is failed to update' : 'Data is failed to create'), "error");
						$('#btn_submit').prop('disabled', false).removeClass('m-loader m-loader--light m-loader--right').text('Submit');
					}
				});
			}
		});
	}
}
