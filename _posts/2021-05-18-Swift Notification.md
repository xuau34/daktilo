---
layout: post
comment: true
title:  "Swift Notification"
subtitle: "an overview"
date:   2021-05-18 11:47:01
categories: [swift]
---		
		
#### NotificaitonCenter

* `default` = per app
* It's used in register an observer and the listener
* Two types for listener -
  * Selector
    * Based on the observer that this will be gone once the observer does not exist
  * Block (Call back)
    * Based on the func. No needs to specify the observer.
    * Should be manually removed the returned NSObjectProtocol from the listening list.
    * Can specify the desired operation queue.
* `addObserver`: listener(queue), `Notification.Name`(id for notification), `object`(secondary id, usually the sender)
* `post` - to the queue that specified in the `addObserver` in **sync** mode.
  * `Notificaiton.Name`
  * `userInfo` - detailed arguments

#### NotificaitonQueue

* `default` = per thread
* Used to **async** post notifications
* `enqueue`
  * `Notification` - the name, object, userInfo
  * `postingStyle`
    * now - sync
    * asap - async
    * whenIdle - async, not yet know the exact definition of idleness
  * `coalesceMask` - combine value; remove the extra calls based on name/object/both.
  * `forModes` - ?
