Manual Controller Augmentation - Alternative 2
==============================================

This is alternative 2, here's `alternative 1`__

__ ./Controller-Augmentation-Alternative-1.html

This is the default scaffolded Controller for the model "User" (without php docs). For Bancha modified parts are highlighted by an ``/*--> */`` in front of the line.

::

    <?php
    class UsersController extends AppController {

        public function index() {
            $this->User->recursive = 0;
    /*--> */    $users = $this->paginate();   // added
    /*--> */    $this->set('users', $users);  // modified, original $this->set('users', $this->paginate());
    /*--> */    return array_merge($this->request['paging']['User'],array('records'=>$users));  // added
        }

        public function view($id = null) {
            $this->User->id = $id;
            if (!$this->User->exists()) {
                throw new NotFoundException(__('Invalid user'));
            }
            $this->set('user', $this->User->read(null, $id));
    /*--> */    return $this->User->data;  // added
        }

        public function add() {
            if ($this->request->is('post')) {
                $this->User->create();

                if ($this->User->save($this->request->data)) {
    /*--> */            if(isset($this->request->params['isBancha']) && $this->request->params['isBancha']) return $this->User->getLastSaveResult();  // added
                    $this->Session->setFlash(__('The user has been saved'));
                    $this->redirect(array('action' => 'index'));
                } else {
    /*--> */            if(isset($this->request->params['isBancha']) && $this->request->params['isBancha']) return $this->User->getLastSaveResult();  // added
                    $this->Session->setFlash(__('The user could not be saved. Please, try again.'));
                }
            }
        }

        public function edit($id = null) {
            $this->User->id = $id;
            if (!$this->User->exists()) {
                throw new NotFoundException(__('Invalid user'));
            }

            if ($this->request->is('post') || $this->request->is('put')) {
                if ($this->User->save($this->request->data['0']['data'])) {
    /*--> */            if(isset($this->request->params['isBancha']) && $this->request->params['isBancha']) return $this->User->getLastSaveResult();  // added
                    $this->Session->setFlash(__('The user has been saved'));
                    $this->redirect(array('action' => 'index'));
                } else {
    /*--> */            if(isset($this->request->params['isBancha']) && $this->request->params['isBancha']) return $this->User->getLastSaveResult();  // added
                    $this->Session->setFlash(__('The user could not be saved. Please, try again.'));
                }
            } else {
                $this->request->data = $this->User->read(null, $id);
            }
        }

        public function delete($id = null) {
            if (!$this->request->is('post')) {
                throw new MethodNotAllowedException();
            }
            $this->User->id = $id;
            if (!$this->User->exists()) {
                throw new NotFoundException(__('Invalid user'));
            }

            if ($this->User->delete()) {
    /*--> */        if(isset($this->request->params['isBancha']) && $this->request->params['isBancha']) return $this->User->getLastSaveResult();  // added
                $this->Session->setFlash(__('User deleted'));
                $this->redirect(array('action'=>'index'));
            }
    /*--> */    if(isset($this->request->params['isBancha']) && $this->request->params['isBancha']) return $this->User->getLastSaveResult();  // added
            $this->Session->setFlash(__('User was not deleted'));
            $this->redirect(array('action' => 'index'));
        }
    }

You can see the changes are relatively small and have two aims:

-  always return a result value
-  don't execute $this->setFlash() and $this->redirect(), since they are
   currently causing problems, also see `How to Expose
   Models <How-to-Expose-Models.html>`_


