<script lang="ts">
    import {
        Button,
        Form,
        FormGroup,
        Input,
        Modal,
        ModalBody,
        ModalFooter,
    } from '@sveltestrap/sveltestrap'

    interface Props {
        isOpen: boolean
        create: (password: string) => void
    }

    let {
        isOpen = $bindable(true),
        create,
    }: Props = $props()
    let password = $state('')
    let field: HTMLInputElement|undefined = $state()
    let validated = $state(false)

    function _save () {
        if (!password) {
            return
        }
        isOpen = false
        create(password)
        password = ''
    }

    function _cancel () {
        isOpen = false
        password = ''
    }
</script>

<Modal toggle={_cancel} isOpen={isOpen} on:open={() => field?.focus()}>
    <Form {validated} on:submit={e => {
        _save()
        e.preventDefault()
    }}>
        <ModalBody>
            <FormGroup floating label="Enter a new password" spacing="0">
                <Input
                    bind:inner={field}
                    type="password"
                    placeholder="New password"
                    required
                    bind:value={password} />
            </FormGroup>
        </ModalBody>
        <ModalFooter>
            <Button
                class="modal-button"
                color="primary"
                on:click={() => validated = true}
            >Create</Button>

            <Button
                class="modal-button"
                color="danger"
                on:click={_cancel}
            >Cancel</Button>
        </ModalFooter>
    </Form>
</Modal>
